# Autoview Sequence Diagrams

This document contains the main runtime sequences for the Autoview application, based on the implementation in the source files of this project.

---

## 1) App initialization sequence

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant Main as autoviewMain
    participant UI as autoviewUI / Xt
    participant OS as X11 / Motif / OS
    participant Ctrl as autoviewController
    participant Buf as GvtkSharedBufferGroup
    participant Ring as GvtkRingBuffer
    participant View as autoviewViewportDisplayer
    participant WL as autoviewWLController
    participant Mag as GvtkSceneMagGlassController
    participant EM as emInitX / emStart

    User->>Main: Launch Autoview
    Main->>OS: Check TTS_FILE and backup trace file
    Main->>Main: Read AV_MAX_IMAGES or set default 256
    Main->>Main: cupInit()
    Main->>Main: createAutoviewController()
    Main->>Ctrl: new autoviewController()

    Main->>UI: XtAppInitialize("Autoview", ...)
    UI-->>Main: UxTopLevel / UxDisplay
    Main->>OS: Load UI resources and labels
    Main->>OS: Load ScanColorRES / ScanFontRES
    Main->>OS: Register converters
    Main->>UI: create_autoviewUI(UxTopLevel)
    UI-->>Main: _avMainInterface
    Main->>UI: XtManageChild + XtPopup
    Main->>OS: wmOccupyAllWorkspaces()

    Main->>Main: initializeAutoviewController()
    Main->>Ctrl: init()

    Ctrl->>Buf: attach("AUTOVIEW")
    Ctrl->>Ring: create(sharedGroup, "autoviewRing")
    Ctrl->>View: new autoviewViewportDisplayer(appContext)
    Ctrl->>View: setConfiguration / create / setImageListAndDraw
    Ctrl->>Ctrl: readAutoWLState()
    Ctrl->>UI: initialize slider and AutoWL toggle
    Ctrl->>WL: new autoviewWLController()
    Ctrl->>Mag: new GvtkSceneMagGlassController(viz)
    Ctrl->>WL: addController()
    Ctrl->>Mag: addController()

    Main->>EM: emInitX(UxAppContext)
    Main->>OS: XSetErrorHandler()
    Main->>OS: write /tmp/autoviewWindowID
    Main->>OS: XtAddEventHandler()
    Main->>Main: registerCallback()
    Main->>EM: emUserEventNotify(...)
    Main->>Main: setInitDone()
    Main->>EM: emStart()
```

---

## 2) Shared buffer image update sequence

```mermaid
sequenceDiagram
    autonumber
    participant Producer as Shared Buffer Producer
    participant Ring as GvtkRingBuffer
    participant CB as autoviewCallbacks::sharedBufferUpdateCB
    participant Ctrl as autoviewController
    participant Shared as GvtkSharedBufferGroup
    participant Meta as updateImageInfo()
    participant View as autoviewViewportDisplayer
    participant Scene as GvtkScene

    Producer->>Ring: write new image(s)
    Ring-->>CB: update notification
    CB->>CB: if getInitDone() == TRUE
    CB->>Ctrl: pullImageFromRingBuffer()
    Ctrl->>Ctrl: emUserEventQ(..., newImageEvent)
    Ctrl-->>CB: return

    CB->>Ctrl: verifyLoadImage()
    Ctrl->>Ctrl: loadImages()
    Ctrl->>Ctrl: updateImages()
    Ctrl->>Ring: getUids(&count)
    Ctrl->>Meta: updateImageInfo(count)

    Meta->>Shared: getBufferByUid(latestUid)
    Shared-->>Meta: latest image buffer
    Meta->>Meta: read preamble: exam_no, ser_no, img_no, gvtk_uid
    Meta->>Meta: update _imgInfo, _startIndex, _endIndex, _imageCount
    Meta-->>Ctrl: metadata updated

    Ctrl->>Ctrl: displayImage(latestUid)
    Ctrl->>View: set image list / draw image
    View->>Scene: update current scene
    View-->>Ctrl: redraw complete
```

---

## 3) Autoview toggle sequence

```mermaid
sequenceDiagram
    autonumber
    participant User
    participant UI as Autoview Toggle Button
    participant CB as autoviewCallbacks
    participant Ctrl as autoviewController
    participant Mag as GvtkSceneMagGlassController

    User->>UI: click Autoview toggle
    UI->>CB: autoviewCB()
    CB->>Ctrl: setAutoviewmode()

    Ctrl->>Ctrl: _autoviewMode = XmToggleButtonGetState(_avAutoviewBtn)

    alt Autoview enabled
        Ctrl->>Ctrl: clear hold image list
        Ctrl->>Ctrl: updateImages()
        Ctrl->>Ctrl: if report cursor is on, turn it off
        Ctrl->>Ctrl: disable Update button
        Ctrl->>Mag: setEnabled(FALSE)
    else Autoview disabled
        Ctrl->>Ctrl: buildTable()
        Ctrl->>Ctrl: _tableUpdateEnable = FALSE
        Ctrl->>Mag: setEnabled(TRUE)
    end
```

---

## 4) Report cursor toggle sequence

```mermaid
sequenceDiagram
    autonumber
    participant User
    participant UI as Report Cursor Toggle
    participant CB as autoviewCallbacks
    participant Ctrl as autoviewController
    participant View as autoviewViewportDisplayer
    participant Cursor as autoviewReportCursorGo
    participant Mag as GvtkSceneMagGlassController
    participant Scene as GvtkScene

    User->>UI: click Report Cursor toggle
    UI->>CB: reportCursorCB()
    CB->>Ctrl: setReportCursorState()

    Ctrl->>Ctrl: _reportCursorOn = XmToggleButtonGetState(_avReportCursorBtn)

    alt Report Cursor ON
        Ctrl->>Ctrl: XmToggleButtonSetState(_avAutoviewBtn, FALSE, FALSE)
        Ctrl->>Ctrl: _autoviewMode = FALSE
        Ctrl->>Ctrl: buildTable()
        Ctrl->>Ctrl: _tableUpdateEnable = FALSE
        Ctrl->>Mag: setEnabled(TRUE)
        Ctrl->>Ctrl: createReportCursorAt(center)
        Ctrl->>Cursor: new autoviewReportCursorGo(...)
        Ctrl->>Cursor: setPosition(_imageWidth)
        Ctrl->>View: draw()
        View->>Scene: render cursor overlay
    else Report Cursor OFF
        Ctrl->>Cursor: setVisible(FALSE)
        Ctrl->>Ctrl: _reportCursor = NULL
        Ctrl->>View: draw()
    end
```

---

## 5) Auto WL toggle sequence

```mermaid
sequenceDiagram
    autonumber
    participant User
    participant UI as Auto WL Toggle
    participant CB as autoviewCallbacks
    participant Ctrl as autoviewController
    participant Scene as GvtkScene
    participant View as autoviewViewportDisplayer

    User->>UI: click Auto WL toggle
    UI->>CB: autoWLCB()
    CB->>Ctrl: setAutoWLState()

    Ctrl->>Ctrl: _autoWLOn = XmToggleButtonGetState(_avAutoWLBtn)
    Ctrl->>Ctrl: writeAutoWLState(_autoWLOn)

    alt Current scene exists
        alt Auto WL enabled
            Ctrl->>Scene: setWindowing(_defaultWindowLevel)
            Ctrl->>View: draw()
            Ctrl->>Ctrl: clearAllWindowing()
        else Auto WL disabled
            Ctrl->>Ctrl: setWLForCurrent(_seriesNo, width, level)
        end
    end
```

---

## 6) Scrollbar image selection sequence

```mermaid
sequenceDiagram
    autonumber
    participant User
    participant UI as Scrollbar
    participant CB as autoviewCallbacks
    participant Ctrl as autoviewController
    participant Table as buildTable()
    participant View as autoviewViewportDisplayer

    User->>UI: drag scrollbar or change value
    UI->>CB: avScrollBarDragCB() / avScrollBarValueChangedCB()
    CB->>Ctrl: sliderDragCB() / sliderValueChangedCB()

    Ctrl->>Ctrl: read slider position
    alt _tableUpdateEnable == TRUE
        Ctrl->>Table: buildTable()
        Ctrl->>Ctrl: _tableUpdateEnable = FALSE
    end

    Ctrl->>Ctrl: _seriesNo = _imgArray[position].serNo

    alt Autoview mode
        Ctrl->>Ctrl: displayImage(_imgArray[position].bufIndex)
    else non-autoview mode
        Ctrl->>Ctrl: displayImage(position)
    end

    Ctrl->>Ctrl: update slider label
    Ctrl->>View: redraw current image
```

---

## 7) Page Up on viewport sequence

```mermaid
sequenceDiagram
    autonumber
    participant User
    participant UI as Viewport keyboard
    participant CB as autoviewCallbacks
    participant Ctrl as autoviewController
    participant Table as buildTable()

    User->>UI: press Page Up
    UI->>CB: pageUpOnViewportCB()
    CB->>Ctrl: pageUpOnViewport()

    Ctrl->>Ctrl: get current slider position
    alt _tableUpdateEnable == TRUE
        Ctrl->>Table: buildTable()
        Ctrl->>Ctrl: _tableUpdateEnable = FALSE
    end
    Ctrl->>Ctrl: walk backward through series group
    alt valid previous position found
        Ctrl->>UI: set scrollbar value
        Ctrl->>Ctrl: sliderValueChangedCB()
    end
```

---

## 8) Page Down on viewport sequence

```mermaid
sequenceDiagram
    autonumber
    participant User
    participant UI as Viewport keyboard
    participant CB as autoviewCallbacks
    participant Ctrl as autoviewController
    participant Table as buildTable()

    User->>UI: press Page Down
    UI->>CB: pageDownOnViewportCB()
    CB->>Ctrl: pageDownOnViewport()

    Ctrl->>Ctrl: get current slider position and max
    alt _tableUpdateEnable == TRUE
        Ctrl->>Table: buildTable()
        Ctrl->>Ctrl: _tableUpdateEnable = FALSE
    end
    Ctrl->>Ctrl: walk forward through series group
    alt valid next position found
        Ctrl->>UI: set scrollbar value
        Ctrl->>Ctrl: sliderValueChangedCB()
    end
```

---

## 9) Up / Down key sequence

```mermaid
sequenceDiagram
    autonumber
    participant User
    participant WL as autoviewWLController
    participant CB as autoviewCallbacks
    participant Ctrl as autoviewController
    participant UI as Scrollbar

    User->>WL: press Up Arrow
    WL->>CB: upKeyOnViewportCB()
    CB->>Ctrl: upKeyOnViewport()

    Ctrl->>UI: read scroll value
    alt value > 0
        Ctrl->>UI: set value = value - 1
        Ctrl->>Ctrl: sliderValueChangedCB()
    end
```

```mermaid
sequenceDiagram
    autonumber
    participant User
    participant WL as autoviewWLController
    participant CB as autoviewCallbacks
    participant Ctrl as autoviewController
    participant UI as Scrollbar

    User->>WL: press Down Arrow
    WL->>CB: downKeyOnViewportCB()
    CB->>Ctrl: downKeyOnViewport()

    Ctrl->>UI: read scroll value and max
    alt value + 1 < max
        Ctrl->>UI: set value = value + 1
        Ctrl->>Ctrl: sliderValueChangedCB()
    end
```

---

## 10) Mouse window / level adjustment sequence

```mermaid
sequenceDiagram
    autonumber
    participant User
    participant WL as autoviewWLController
    participant Scene as GvtkScene
    participant CB as autoviewCallbacks
    participant Ctrl as autoviewController
    participant View as autoviewViewportDisplayer

    User->>WL: press mouse BUTTON2_MASK
    WL->>WL: avLockImages(TRUE)

    User->>WL: release mouse BUTTON2_MASK
    WL->>Scene: get current windowing()
    WL->>CB: setWindowLevel(&win)
    CB->>Ctrl: setWL(&win)

    Ctrl->>Ctrl: if _autoWLOn == FALSE
    Ctrl->>Ctrl: set WL for current series
    WL->>CB: avDisplayBlockedImages()
    CB->>Ctrl: displayBlockedImages()
    Ctrl->>View: redraw scene
    WL->>CB: avLockImages(FALSE)
```

---

## 11) Mouse button 1 turns off Autoview

```mermaid
sequenceDiagram
    autonumber
    participant User
    participant WL as autoviewWLController
    participant CB as autoviewCallbacks
    participant Ctrl as autoviewController

    User->>WL: mouse release with BUTTON1_MASK
    WL->>CB: setAutoviewOff()
    CB->>Ctrl: setAVButton(FALSE)

    Ctrl->>Ctrl: _autoviewMode = FALSE
    Ctrl->>Ctrl: buildTable()
    Ctrl->>Ctrl: _tableUpdateEnable = FALSE
```

---

## 12) Desktop move / maximize / minimize sequence

```mermaid
sequenceDiagram
    autonumber
    participant External as Desktop Manager / Window Manager
    participant Ctrl as autoviewController
    participant UI as Autoview Main Window
    participant OS as X11

    External->>Ctrl: autoviewMove(desktop, x, y)
    Ctrl->>Ctrl: evaluate current desktop state
    alt target position is home or GRx positions
        Ctrl->>Ctrl: update _desktopInfo[desktop]
        alt window is currently active desktop
            Ctrl->>UI: maximize() or minimize() or setAvSize()
            Ctrl->>OS: XMoveWindow(...)
        end
    else non-home/non-GRx position
        Ctrl->>Ctrl: set forced minimize if maximized
        alt active desktop
            Ctrl->>UI: minimize() if needed
            Ctrl->>OS: XMoveWindow(...)
        end
        Ctrl->>Ctrl: update _desktopInfo[desktop]
    end
```

---

## 13) Auto WL state persistence sequence

```mermaid
sequenceDiagram
    autonumber
    participant Ctrl as autoviewController
    participant OS as File System
    participant State as AV state file

    Ctrl->>Ctrl: writeAutoWLState(state)
    Ctrl->>OS: getenv("AV_STATE_FILE")
    alt env var missing
        Ctrl->>OS: use /usr/g/w/state/avStateFile
    end
    Ctrl->>State: fopen(..., "w")
    Ctrl->>State: fprintf("AUTOWL=TRUE/FALSE")
    Ctrl->>State: fflush + fclose
```

```mermaid
sequenceDiagram
    autonumber
    participant Ctrl as autoviewController
    participant OS as File System
    participant State as AV state file

    Ctrl->>Ctrl: readAutoWLState()
    Ctrl->>OS: getenv("AV_STATE_FILE")
    alt env var missing
        Ctrl->>OS: use /usr/g/w/state/avStateFile
    end
    Ctrl->>State: fopen(..., "r")
    Ctrl->>State: fgets(...)
    Ctrl->>Ctrl: parse value as TRUE/FALSE
    Ctrl->>Ctrl: set _autoWLOn
```

---

## Notes

- These diagrams reflect the runtime behavior implemented in the Autoview project files.
- The sequences are intentionally split by feature, so each diagram represents one specific flow.
- Mermaid is supported in many Markdown renderers and GitHub viewers, making this file suitable for review and download.
