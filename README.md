
Hackathon Pitch – Shape-from-Shading Solution


🔍 Problem: 
Current lunar terrain reconstruction techniques using Shape-from-Shading (SfS) assume the Sun’s direction is known and accurate. However, this metadata is often noisy, missing, or incorrect — especially in older or polar mission datasets. This leads to inaccurate DEMs (Digital Elevation Models), especially in shadowed or complex lighting areas.

💡 Our Idea (Cause → Effect):
We propose a method to **self-estimate the Sun’s direction directly from the image itself**, rather than blindly trusting the metadata. This allows us to apply SfS reliably even when the original data is missing, faulty, or distorted. As a result, we can generate **more accurate DEMs** using only a single mono image, with improved robustness to lighting errors and surface albedo variation.


1. Rough Idea (Pipeline)

1. **Input & Preprocessing**
   - Load mono lunar images (TMC-2, LRO NAC) with any available Sun metadata
   - Clean the image: remove striping/noise, convert brightness to reflectance

2. **Self-Calibrating Sun Direction Estimation** ✅ *Novel Block*
   a. **Brightest Pixel Heuristic**: Gradually increase brightness until top 0.1% pixels saturate → use their location to estimate Sun direction
   b. **Shadow Geometry Refinement**: Measure length/angle of crater shadows to fine-tune Sun elevation angle
   c. **Optional Tiny CNN**: Train a small neural network on clean images with known lighting to predict Sun azimuth/elevation automatically

3. **Albedo-Aware Correction**
   - Detect unusually bright regions that might indicate high-albedo (not slope) and mask them to avoid slope errors

4. **Shape-from-Shading Core**
   - Use the corrected Sun direction + cleaned image to run a classical SfS algorithm
   - Outputs high-resolution relative elevation map and surface normals

5. **Anchor to Absolute Heights**
   - Tie the result to LOLA or SLDEM coarse DEMs to convert relative shape to absolute elevations

6. **Visualization**
   - Package the result as a DEM GeoTIFF and 3D hillshade view
   - Optionally display in QGIS or browser for demo purposes

2. USP (Uniqueness)

✅ We break the field’s assumption: “Sun direction is known.”
✅ We work with just **a single image** — no stereo required.
✅ We build an **adaptive and self-correcting SfS module**.
✅ Our pipeline handles **albedo variations** and shadows better.
✅ The tool can be integrated into existing QGIS or mission pipelines.
✅ It is light enough to run on laptops and even onboard systems.

3. Plan for Hackathon

Goal: Submit the **idea with a proof-of-concept pipeline outline**. Actual implementation can follow.

| Day | Task |
|-----|------|
| Day 1 | Finalize data format, test image brightness scaling, draft Sun estimation logic |
| Day 2 | Validate crater shadow-based refinement; sketch CNN plan (optional); write up assumptions |
| Day 3 | Assemble visuals, pipeline diagram, and presentation with technical explanation |

Data required: 2–3 high-quality LRO NAC mono images with visible craters + LOLA DEM samples


4. Feasibility (Reality Check)

✅ Yes — the idea is grounded in current challenges
✅ A demo of the Sun direction estimation method alone would be valuable
✅ SfS implementation can use existing solvers — not reinvented
✅ Success = show your Sun estimation improves DEM results or shadow alignment

⚠️ Watch out for:
- High-albedo confusion (can be handled with masking)
- Shadow edge accuracy (choose clear crater examples)
- Time limit: skip CNN if needed, focus on core idea

🎯 Final Verdict:
This is a **real, novel, and achievable research-oriented solution**. It addresses a serious gap in current planetary imaging pipelines and could lead to publishable work or adoption by agencies if refined further.
