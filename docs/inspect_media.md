# Media Inspection

Once a media file is successfully uploaded using the `POST /v1/media` endpoint, you can initiate deepfake analysis through the **"Inspect Media"** action in the ProbeTruth interface.

This process automatically triggers backend analysis using multiple AI models, depending on the type of uploaded media (video, audio, or image). No additional API call or input parameters are required at this step.

---

## Workflow

1. **Upload Media**  
   Upload a video, audio, or image file using the `/v1/media` endpoint.

2. **Inspect Media**  
   After upload, click the **"Inspect Media"** button in the ProbeTruth dashboard. This triggers backend analysis.

3. **Model Processing**  
   Based on the type of media:
   - Visual models are run for images/videos.
   - Audio models for audio clips.
   - Audiovisual models for eligible media types.
   
   Each model processes the file in parallel or sequentially, depending on configuration.

4. **Live Feedback**  
   - During analysis, a status bar or loading icon is shown.
   - Once complete, the frontend displays predictions and confidence scores.

---

## Backend Behavior

- The system automatically selects the appropriate models based on the uploaded media type.
- No additional input parameters are required.
- The analysis status is updated in real time.

---

## Example Response (GUI Display)

| Model             | Confidence | Verdict     |
|------------------|------------|-------------|
| VisualNet V2     | 93.2%      | Deepfake    |
| AudioAnalyzer X  | 98.7%      | Authentic   |
| Fusion AI        | 95.1%      | Deepfake    |

> Final Decision: **Deepfake Detected**

---

## Next Step: Generate Report

Once the inspection is complete, users can click on the **"Generate Report"** button to receive a downloadable PDF forensic report containing:

- Metadata
- Model decisions
- Confidence scores
- Visual evidence (if available)

