# Image Storage Fundamentals

## 1. How Bytes Store an Image

At the lowest level, an image is just **a sequence of bytes (binary data)**.

---

### 1.1 Pixel Representation

An image is a grid of pixels:

- Resolution: `width × height`
- Each pixel stores color information

Most common format:

- **RGB (3 channels)**
  - R (Red)
  - G (Green)
  - B (Blue)

Each channel is typically:

- **1 byte (8 bits)** → value range: `0–255`

---

### 1.2 Example

A single pixel:

[255, 0, 0] → Red

Stored in binary:

11111111 00000000 00000000

So:

- 1 pixel = **3 bytes (24 bits)**

---

### 1.3 Full Image in Memory (Raw Data)

Image size (raw) ≈ width × height × channels

Example:

1920 × 1080 × 3 ≈ 6 MB

This is **uncompressed size**.

---

## 2. What an Image File Actually Stores

Image formats (PNG, JPEG, GIF) do NOT usually store raw pixels directly.

They contain:

### 2.1 Metadata

- Width / Height
- Color format (RGB, grayscale, etc.)
- Compression type
- Additional info (EXIF, etc.)

### 2.2 Compressed Pixel Data

Instead of storing raw pixels:

pixel1, pixel2, pixel3, ...

They store:

compressed representation of pixel patterns

---

## 3. Why Same Resolution ≠ Same File Size

Even if two images have:

- Same resolution (e.g. 1920×1080)
- Same format (e.g. PNG)

Their file sizes can still be very different.

---

## 4. Key Reason: Compression Efficiency

### 4.1 Simple Image (Highly Compressible)

Example:

- White background
- Large uniform areas

Compression can encode it like:

"repeat white many times"

Result:

- Very small file

---

### 4.2 Complex Image (Hard to Compress)

Example:

- Photo with textures, noise, details

Pixels vary a lot:

[123, 98, 76], [120, 95, 70], ...

Result:

- Hard to find patterns
- Larger file

---

## 5. Format Differences

### 5.1 PNG (Lossless)

- Compression: Yes
- Data loss: No

### 5.2 JPEG (Lossy)

- Compression: Yes
- Data loss: Yes

### 5.3 GIF

- Limited to 256 colors
- Suitable for simple graphics

---

## 6. Key Insight

Image size depends on:

- Resolution
- Compression algorithm
- Image complexity
- Format
- Quality settings

---

## 7. One-Line Summary

Images are stored as compressed pixel data, and file size depends more on compressibility than resolution alone.
