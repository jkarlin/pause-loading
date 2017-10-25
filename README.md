# The PauseLoading API
The PauseLoading API allows current network requests in a frame (and its children) to continue but queues up all new requests. Those requests will be started once the frame is unpaused.

Example:

iframe that has its loads paused:
```javascript
window.addEventListener("message", receiveMessage, false);

function receiveMessage(event)
{
  if (e.data === "LoadResource") {
    fetch("http://example.com/foo").then(r => console.log("Fetching"));
  }
}
```

Parent frame:
```javascript
var frameElement = document.getElementById("hungryFrameID");
await frameElement.pauseLoading();
console.log("Frame will queue new requests");

frameElement.contentWindow.postMessage("LoadResource");

await frameElement.resumeLoading();
console.log("All loads resumed");
```

Will print to the console:
```text
Frame will queue new requests
All loads resumed
Fetching
```

## Use Cases

1. The [TransferSizePolicy](https://github.com/WICG/transfer-size-policy) API notifies frames when their child frames have used more network data than desired. PauseLoading is a response that the parent frame could use to limit the frame without completely breaking it.

1. This could also be used to reduce the overhead of low-priority frames (e.g., those that are offscreen or not focused).

## Details


## TBD
1. How is the frame alerted that its network has been paused? 
2. Is there a per-frame pause counter?
