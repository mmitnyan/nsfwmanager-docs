# Asynchronous Loading

NSFW Manager keeps the user interface responsive by offloading I/O and compute-intensive work to background threads.

## Goals

<!-- Why asynchronous loading matters for a desktop scanning application. -->

## Threading Model

<!-- High-level description of the threading strategy: UI thread, worker thread pool, task queue. -->

## Thumbnail and Preview Loading

<!-- How thumbnails are loaded lazily as the user scrolls through results. -->

## Scan Progress Reporting

<!-- How progress updates are marshalled back to the UI thread safely. -->

## Cancellation

<!-- How in-flight scan operations are cancelled when the user stops a scan. -->

## Error Handling

<!-- How errors surfaced on background threads are reported to the user. -->
