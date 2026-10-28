# Voyage


<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Media Evidence Review — Starter</title>
  <style>
    :root {
      color-scheme: dark;
      font-family: system-ui, sans-serif;
      background: #101820;
      color: #e9f0f3;
    }

    * { box-sizing: border-box; }
    body { margin: 0; }
    main { max-width: 900px; margin: auto; padding: 32px 20px; }
    p { line-height: 1.5; color: #bdcbd1; }

    .panel {
      margin-top: 20px;
      padding: 20px;
      border: 1px solid #30424c;
      border-radius: 14px;
      background: #17232c;
    }

    .grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 16px;
    }

    label { display: block; margin-bottom: 7px; font-weight: 650; }

    input, button {
      width: 100%;
      padding: 11px;
      border: 1px solid #425762;
      border-radius: 8px;
      background: #101820;
      color: #e9f0f3;
      font: inherit;
    }

    button {
      margin-top: 18px;
      border: 0;
      background: #18a999;
      color: #062522;
      font-weight: 750;
      cursor: pointer;
    }

    button:disabled { opacity: .6; cursor: wait; }
    .hint { margin-top: 6px; font-size: .9rem; color: #9fb1b9; }
    .notice { padding: 10px 14px; border-left: 4px solid #e8b04b; }
    #status { min-height: 1.5em; color: #8fe1d7; }

    @media (max-width: 650px) {
      .grid { grid-template-columns: 1fr; }
      main { padding: 22px 14px; }
    }
  </style>
</head>
<body>
<main>
  <h1>Media Evidence Review</h1>
  <p>
    Starter project: review an image or video, analyze selected frames,
    and compare them with reference images.
  </p>

  <p class="notice">
    Analysis results are leads, not proof. A model score or visual match
    cannot establish who created or shared media.
  </p>

  <section class="panel">
    <div class="grid">
      <div>
        <label for="media">Image or video to review</label>
        <input id="media" type="file" accept="image/*,video/*">
      </div>

      <div>
        <label for="references">Optional reference images</label>
        <input id="references" type="file" accept="image/*" multiple>
      </div>

      <div>
        <label for="model">Classifier model ID</label>
        <input id="model" type="text" placeholder="publisher/model-name">
        <p class="hint">Choose a model that can run in the browser.</p>
      </div>

      <div>
        <label for="fakeLabels">Labels that indicate manipulation</label>
        <input id="fakeLabels" type="text" value="fake, manipulated, synthetic">
      </div>

      <div>
        <label for="frameCount">Video frames to sample</label>
        <input id="frameCount" type="number" min="1" max="16" value="8">
      </div>

      <div>
        <label for="distance">Reference-match threshold</label>
        <input id="distance" type="number" min="0" max="64" value="10">
      </div>
    </div>

    <p class="hint">
      Decide how to load the model and process media. Keep selected media
      in the browser unless your team intentionally designs otherwise.
    </p>

    <button id="analyze">Analyze media</button>
    <p id="status" role="status" aria-live="polite"></p>
  </section>

  <section id="results" class="panel" hidden>
    <h2>Review results</h2>
    <div id="resultContent"></div>
  </section>
</main>

<script type="module">
  // TODO: Import a browser-compatible inference library and classifier pipeline.
  // Example library: Transformers.js
  // import { pipeline } from "https://cdn.jsdelivr.net/npm/@huggingface/transformers";

  const mediaInput = document.querySelector("#media");
  const referencesInput = document.querySelector("#references");
  const modelInput = document.querySelector("#model");
  const labelsInput = document.querySelector("#fakeLabels");
  const frameCountInput = document.querySelector("#frameCount");
  const distanceInput = document.querySelector("#distance");
  const analyzeButton = document.querySelector("#analyze");
  const status = document.querySelector("#status");
  const results = document.querySelector("#results");
  const resultContent = document.querySelector("#resultContent");

  function setStatus(message) {
    status.textContent = message;
  }

  function validateInputs() {
    const file = mediaInput.files[0];
    const modelId = modelInput.value.trim();
    const frameCount = Number(frameCountInput.value);
    const threshold = Number(distanceInput.value);
    const labels = labelsInput.value
      .split(",")
      .map(label => label.trim().toLowerCase())
      .filter(Boolean);

    if (!file) throw new Error("Choose an image or video.");
    if (!modelId) throw new Error("Enter a classifier model ID.");
    if (!Number.isInteger(frameCount) || frameCount < 1 || frameCount > 16) {
      throw new Error("Frame count must be between 1 and 16.");
    }
    if (!Number.isInteger(threshold) || threshold < 0 || threshold > 64) {
      throw new Error("Threshold must be between 0 and 64.");
    }
    if (labels.length === 0) throw new Error("Enter at least one label.");

    return { file, modelId, frameCount, threshold, labels };
  }

  // TODO 1: Load and cache a browser-compatible image classifier.
  async function loadClassifier(modelId) {
    throw new Error("TODO: connect the classifier library and load the model.");
  }

  // TODO 2: Decode an image directly or sample frames from a video.
  async function extractFrames(file, maximumFrames) {
    throw new Error("TODO: implement image/video frame extraction.");
  }

  // TODO 3: Run inference and calculate a score using the chosen labels.
  async function classifyFrames(classifier, frames, labels) {
    throw new Error("TODO: run the model and interpret its class labels.");
  }

  // TODO 4: Compare sampled frames with selected reference images.
  async function compareReferences(frames, referenceFiles, threshold) {
    throw new Error("TODO: implement reference-image similarity matching.");
  }

  function showStarterResult(file) {
    resultContent.replaceChildren();

    const message = document.createElement("p");
    message.textContent =
      `Selected: ${file.name}. Complete the TODOs to display analysis results.`;
    resultContent.append(message);
    results.hidden = false;
  }

  analyzeButton.addEventListener("click", async () => {
    analyzeButton.disabled = true;
    results.hidden = true;

    try {
      const options = validateInputs();
      setStatus("Starter is ready. Implement the analysis TODOs to continue.");
      showStarterResult(options.file);

      // Suggested workflow:
      // const classifier = await loadClassifier(options.modelId);
      // const frames = await extractFrames(options.file, options.frameCount);
      // const scores = await classifyFrames(classifier, frames, options.labels);
      // const matches = await compareReferences(
      //   frames, [...referencesInput.files], options.threshold
      // );
      // TODO 5: Render scores, matches, and clear limitations in the report.
    } catch (error) {
      setStatus(`Unable to continue: ${error.message}`);
    } finally {
      analyzeButton.disabled = false;
    }
  });
</script>
</body>
</html>
