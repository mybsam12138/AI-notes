# Video Storage and Compression Fundamentals

## 1. What is Video in Data Terms

A video is fundamentally:

- A sequence of images (frames)
- Played over time (FPS - frames per second)

Example:

Video = frames + timing

---

## 2. Raw Video Representation (Uncompressed)

Each frame is an image:

- Resolution: width × height
- Channels: usually RGB (3 channels)

Raw size formula:

size ≈ width × height × channels × number_of_frames

Example:

1920 × 1080 × 3 × 30 fps ≈ 180 MB per second

👉 Raw video is extremely large and impractical to store.

---

## 3. Why Compression is Needed

In real-world video:

- Most pixels do NOT change between frames
- Only small regions (objects, people) move

👉 Compression exploits this redundancy.

---

## 4. Key Idea: Video is Stored as Changes, Not Full Frames

Instead of storing every frame fully:

Video = key frames + differences between frames

---

## 5. Frame Types

### 5.1 I-frame (Intra Frame)

- Full image (like a JPEG)
- Independent
- Largest size

Used for:
- Starting points
- Scene changes

---

### 5.2 P-frame (Predicted Frame)

- Stores difference from previous frame
- Uses motion estimation

Contains:
- Motion vectors (how blocks move)
- Residual (small pixel differences)

---

### 5.3 B-frame (Bidirectional Frame)

- Uses both previous and future frames
- Most compressed

---

## 6. How Compression Works (Step-by-Step)

### Step 1: Capture frame

Camera sensor captures real frames continuously.

---

### Step 2: Block division

Frame is divided into small blocks (e.g. 16×16 pixels).

---

### Step 3: Motion estimation

Encoder finds similar blocks in previous (or future) frames.

Output:
- Motion vectors (dx, dy)

---

### Step 4: Residual calculation

Residual = actual block - predicted block

---

### Step 5: Transform and quantization

- Convert residual to frequency domain
- Reduce precision (lossy step)

---

### Step 6: Entropy encoding

Compress data using efficient encoding (like ZIP-style methods).

---

## 7. What is Stored in Video Bytes

Instead of raw pixels, video stores:

- I-frames (full images)
- Motion vectors
- Residual data
- Compression metadata

---

## 8. Decoder Process (Playback)

To reconstruct frames:

1. Decode previous frame
2. Apply motion vectors
3. Add residual

👉 Rebuild current frame

---

## 9. Why Same Resolution ≠ Same File Size

Even if videos have:

- Same resolution
- Same FPS

File size depends on:

- Motion complexity
- Scene changes
- Noise / detail
- Compression settings (bitrate)

Example:

- Static scene → small file
- Fast action → large file

---

## 10. Key Insight

> Video compression reduces size by storing full frames occasionally and representing most frames as motion + differences.

---

## 11. One-Line Summary

Video is stored as a combination of key frames and compressed changes between frames, using motion estimation and efficient encoding to reduce data size.
