# Downloads example

This repo has KT (test) prep content and analyzes on particular async example in this README.

## Output from the Exercise

```
DownloadAsync_Task: Started downloading 'https://github.com/progit/progit2/releases/download/2.1.360/progit.pdf'
DownloadAsync_Task gave control back to caller
DownloadAsync_Task completed work

====================== DownloadAsync_Await ======================
DownloadAsync_Await: Started downloading 'https://github.com/progit/progit2/releases/download/2.1.360/progit.pdf'
DownloadAsync_Task: Saved to 'file.pdf'
DownloadAsync_Await: Downloaded 'https://github.com/progit/progit2/releases/download/2.1.360/progit.pdf'
DownloadAsync_Await: Saved 'progit_await.pdf'
DownloadAsync_Await gave control back to caller
DownloadAsync_Await completed work

======================= DownloadMultipleAsync =======================
DownloadAsync_Await: Started downloading 'https://github.com/progit/progit2/releases/download/2.1.360/progit.pdf'
DownloadMultipleAsync: DownloadAsync_Await of 'https://github.com/progit/progit2/releases/download/2.1.360/progit.pdf' started
DownloadAsync_Await: Started downloading 'https://github.com/SoftUni/Programming-Basics-Book-CSharp-EN/raw/b153f06082ede1d0e4c8e7994c4d6a628886c22e/resources/Programming-Basics-CSharp-Book-and-Video-Lessons-Nakov-v2019.pdf'
DownloadMultipleAsync: DownloadAsync_Await of 'https://github.com/SoftUni/Programming-Basics-Book-CSharp-EN/raw/b153f06082ede1d0e4c8e7994c4d6a628886c22e/resources/Programming-Basics-CSharp-Book-and-Video-Lessons-Nakov-v2019.pdf' started
DownloadAsync_Await: Downloaded 'https://github.com/progit/progit2/releases/download/2.1.360/progit.pdf'
DownloadAsync_Await: Saved 'progit_multi_1.pdf'
DownloadAsync_Await: Downloaded 'https://github.com/SoftUni/Programming-Basics-Book-CSharp-EN/raw/b153f06082ede1d0e4c8e7994c4d6a628886c22e/resources/Programming-Basics-CSharp-Book-and-Video-Lessons-Nakov-v2019.pdf'
DownloadAsync_Await: Saved 'programming_basics_multi_2.pdf'
DownloadMultipleAsync: DownloadAsync_Await of all files completed
DownloadMultipleAsync gave control back to caller
DownloadMultipleAsync completed work
```

## Analysis

# Analysis of Download Methods: Async, Await, and Task

## 1. **`DownloadSync`**
- **Synchronous**: Blocks the thread during both download and save operations.
- **Thread Usage**: Inefficient as the thread is occupied throughout.
- **Use Case**: Simple applications where blocking is acceptable.
- **Pros**: Simple and easy to understand.
- **Cons**: Inefficient for large or I/O-heavy workloads.

## 2. **`DownloadAsync_Task`**
- **Task-Based**: Returns a `Task<byte[]>`, deferring all subsequent operations to the caller.
- **Non-Blocking**: Does not block the thread during download.
- **Use Case**: When the caller wants control over subsequent steps (e.g., saving the file).
- **Pros**: Lightweight, gives caller flexibility.
- **Cons**: Caller must handle chaining or awaiting.

## 3. **`DownloadAsync_Await`**
- **Fully Asynchronous**: Uses `await` for both downloading and saving.
- **Thread Efficiency**: Thread is released during both I/O-bound operations.
- **Use Case**: When a single, fully asynchronous method is desired for both download and save.
- **Pros**: Clean, readable, fully non-blocking.
- **Cons**: Less control for the caller; handles saving internally.

## 4. **`DownloadMultipleAsync`**
- **Concurrent Execution**: Uses `Task.WhenAll` to run downloads concurrently.
- **Thread Efficiency**: Threads are released during all operations.
- **Use Case**: Efficiently handling multiple downloads.
- **Pros**: Reduces total execution time with parallelism.
- **Cons**: More complex than single-task methods.

# Comparison Table

| **Aspect**            | **`DownloadSync`**          | **`DownloadAsync_Task`**    | **`DownloadAsync_Await`**   | **`DownloadMultipleAsync`**   |
|------------------------|-----------------------------|-----------------------------|-----------------------------|-------------------------------|
| **Thread Blocking**   | Yes                         | No                          | No                          | No                            |
| **Parallelism**       | None                        | None                        | None                        | Yes                           |
| **Control Flow**      | Caller blocks until done.   | Caller handles subsequent.  | Fully managed in method.    | Handles concurrent tasks.     |
| **Complexity**        | Simple                      | Moderate                    | Simple                      | Complex for multiple tasks.   |
| **Use Case**          | Simple blocking apps.       | Non-blocking download only. | Fully async single task.    | Efficient multi-downloads.    |

# Summary

- **Single Downloads**: Use `DownloadAsync_Await` for simplicity or `DownloadAsync_Task` for caller-controlled workflows.
- **Multiple Downloads**: Use `DownloadMultipleAsync` for concurrent execution.
- **Simple Needs**: Use `DownloadSync` for straightforward blocking behavior, though it's inefficient for I/O-heavy tasks.
