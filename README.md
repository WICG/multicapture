## Introduction

Some applications wish to concurrently capture multiple surfaces.

Capturing multiple surfaces is doable using existing APIs - it is possible to call `getDisplayMedia()` multiple times. However, this is not very ergonomic, and creates **serious friction for the user**:

1. The user has to interact with the browser's media-picker multiple times.
2. The user has to interact with the application multiple times, signaling that they want to capture yet another surface, and providing a new transient activation each time.
3. The user is liable to make mistakes when trying to remember which surfaces they've already started capturing, and which surfaces remain for them to capture.

Ideally, a single transient activation could be used for single API invocation, providing the user with a media-picker with functionality **akin** to checkboxes (mentioned here by way of example; we don't need to mandate specific UX elements). The user would be allowed to choose all of the display surfaces that they want to capture, then click OK once. It is clear from context that these are all of e surfaces the user was aiming to capture, and that no additional API calls to gDM or the like are necessary.

Illustration:
![mock](https://user-images.githubusercontent.com/22117736/156051996-dd3a382f-c7b7-4180-b354-70fc7a251744.jpeg)


## Use Cases

### Use-case 1: Streamers presenting multiple surfaces (dynamic receivers)

Consider an instructor presenting multiple tabs to several students.
* Instructor streams multiple tabs to an SFU.
* Individual students independently choose tab to view at any given moment.

With a single click, the instructor can start capturing all the relevant tabs.

### Use-case 2: Streamers presenting changing surface (dynamic sender)

Video conferencing software asks the user to choose all the tabs the user wishes to share.
The application captures all surfaces, but, at any given moment, it only relays to the SFU a single tab. Which tab is relayed depends on app-specific logic. (For instance, maybe only the last-active tab.)

### Use-case 3: Record N screens/windows/tab

Recording for compliance/training/billing reasons.

### Use-case 4: Record and compose

Record multiple windows.
Redraw them to a canvas to produce a video of a virtual desktop.
This virtual desktop only has the captured windows, which improves privacy a lot over what users currently do nowadays - sharing the entire (real) desktop so as to share a handful of windows.
(Additional, orthogonal API for learning the position and size of windows needed to make this truly powerful.)

## Goals

Provide an API which allows multiple screen-captures to be initiated. It should only require a single transient activation. Ideally, the user agent should present to the user a UX which would render certain user-mistakes impossible (e.g. capturing the same surface multiple times).

## Possible Solutions

### Possible API 1: New method (getDisplayMediaSet)
```webidl
partial interface MediaDevices {
  Promise<sequence<MediaStream>> getDisplayMediaSet(
    optional MediaStreamConstraints constraints = {});
}
```

### Possible API 2: Overloaded return type for getDisplayMedia
Add a possible paramter to `getDisplayMedia` called `maxSurfaces`.
Its default value is 1. With that value, the existing behavior is manifested.
For values greather than 1, the new behavior is manifested (multi-picker), and the return type changes to `Promise<sequence<MediaStream>>`.

## Examples

See mock above.
