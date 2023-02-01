<script lang="ts">
  import "./app.css";

  let password = "";
  let characterLength = 16;
  let useUppercaseLetter = false;
  let useLowercaseLetter = true;
  let useNumbers = false;
  let useSymbols = false;
  let copied = false;

  const lowercaseLetter = "abcdefghijklmnopqrstuvwxyz";
  const uppercaseLetter = lowercaseLetter.toUpperCase();
  const numbers = "0123456789";
  const symbols = "!@#$%^&*()_+~|}{[]\\:;?><,./-=";

  generatePassword();

  function generatePassword() {
    password = "";
    let charset = "";

    if (useUppercaseLetter) charset += uppercaseLetter;
    if (useLowercaseLetter) charset += lowercaseLetter;
    if (useNumbers) charset += numbers;
    if (useSymbols) charset += symbols;

    for (let i = 0, n = charset.length; i < characterLength; ++i) {
      password += charset.charAt(Math.floor(Math.random() * n));
    }
  }
  function copyPassword() {
    navigator.clipboard.writeText(password);
    copied = true;
    setTimeout(() => (copied = false), 2000);
  }

  $: console.log("yeah");
</script>

<main>
  <div class="title">
    <h1>Password Generator</h1>
  </div>
  <div class="password-viewer">
    <div class="password">
      <span>{password}</span>
    </div>
    <div class="copy-icon">
      <button on:click={copyPassword}>
        {#if copied}
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="20"
            height="20"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            class="feather feather-check"><polyline points="20 6 9 17 4 12" /></svg
          >
        {:else}
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="20"
            height="20"
            viewBox="0 0 24 24"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            class="feather feather-copy"
            ><rect x="9" y="9" width="13" height="13" rx="2" ry="2" /><path
              d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"
            /></svg
          >
        {/if}
      </button>
    </div>
  </div>
  <div class="password-config">
    <div class="character-length">
      <span>Character Length</span>
      <span>{characterLength}</span>
    </div>
    <div class="character-input-range">
      <input
        type="range"
        name="input-password-range"
        id="input-password-range"
        bind:value={characterLength}
        min="5"
        max="27"
        on:input={generatePassword}
      />
    </div>
    <div class="password-options">
      <div class="password-option">
        <label for="use-uppercase-letter"
          >Include Uppercase Letters
          <input type="checkbox" bind:checked={useUppercaseLetter} id="use-uppercase-letter" />
          <span class="checkmark" />
        </label>
      </div>
      <div class="password-option">
        <label for="use-lowercase-letter"
          >Include Lowercase Letters
          <input type="checkbox" bind:checked={useLowercaseLetter} id="use-lowercase-letter" />
          <span class="checkmark" />
        </label>
      </div>
      <div class="password-option ">
        <label for="use-numbers"
          >Include Numbers
          <input type="checkbox" bind:checked={useNumbers} id="use-numbers" />
          <span class="checkmark" />
        </label>
      </div>
      <div class="password-option">
        <label for="use-symbols"
          >Include Symbols
          <input type="checkbox" bind:checked={useSymbols} id="use-symbols" />
          <span class="checkmark" />
        </label>
      </div>
    </div>
    <div class="password-generate-button">
      <button type="submit" on:click={generatePassword}>Generate</button>
    </div>
  </div>
</main>

<style>
  main {
    display: flex;
    flex-direction: column;
    gap: 20px;
    min-width: 90vw;
    max-width: 100vw;
  }

  @media screen and (min-width: 400px) {
    main {
      min-width: 350px;
    }
  }

  .password-viewer,
  .password-config {
    padding: 25px;
  }

  .title {
    font-size: 0.5rem;
    color: var(--foreground-color-secondary);
    text-align: center;
  }

  .password-viewer,
  .password-config {
    background-color: var(--background-color-secondary);
  }

  .password-viewer {
    display: flex;
    justify-content: space-between;
    width: 100%;
    min-height: 20px;
  }

  .password-viewer .password {
    padding-right: 8px;
    font-size: 1.2rem;
  }

  .password-viewer .copy-icon {
    padding-left: 8px;
  }

  .password-viewer .copy-icon button {
    background-color: transparent;
    border: none;
    cursor: pointer;
  }

  .password-viewer .copy-icon button svg {
    stroke: var(--main-color-primary);
    fill: none;
    transition: 0.2s;
  }

  .password-viewer .copy-icon button:hover svg {
    transform: scale(1.1);
  }

  .password-config {
    display: flex;
    flex-direction: column;
    gap: 20px;
    user-select: none;
  }

  .password-config .character-length {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .password-config .character-input-range input[type="range"] {
    appearance: none;
    background: var(--background-color-primary);
    border: 2px solid var(--background-color-primary);
    width: 100%;
    height: 10px;
    cursor: pointer;
  }

  .password-config .character-input-range input[type="range"]::-webkit-slider-thumb {
    appearance: none;
    height: 25px;
    width: 25px;
    border-radius: 50%;
    background: var(--foreground-color-primary);
    border: 2px solid var(--foreground-color-primary);
    cursor: pointer;
  }

  .password-config .character-input-range input[type="range"]::-ms-thumb {
    box-shadow: 1px 1px 1px #000000, 0px 0px 1px #0d0d0d;
    border: 1px solid #000000;
    height: 36px;
    width: 16px;
    border-radius: 3px;
    background: #ffffff;
    cursor: pointer;
  }

  .password-config .password-options {
    display: flex;
    flex-direction: column;
  }

  .password-config .password-options .password-option {
    position: relative;
    display: flex;
    flex-direction: row-reverse;
    justify-content: flex-end;
    align-items: center;
  }

  .password-config .password-options .password-option label {
    width: 100%;
    height: 100%;
    padding: 10px 0 10px 30px;
    cursor: pointer;
  }

  .password-config .password-options .password-option input {
    position: absolute;
    opacity: 0;
    width: 0;
    height: 0;
  }

  .password-config .password-options .password-option .checkmark {
    position: absolute;
    left: 0;
    width: 20px;
    height: 20px;
    background-color: transparent;
    border: 2px solid var(--background-color-primary);
  }

  .password-config .password-options .password-option .checkmark:after {
    content: "";
    position: absolute;
    display: none;
    left: 5px;
    top: 1px;
    width: 4px;
    height: 8px;
    border: 2px solid var(--background-color-primary);
    border-width: 0 3px 3px 0;
    transform: rotate(45deg);
  }

  .password-config .password-options .password-option:hover input ~ .checkmark {
    background-color: var(--background-color-primary);
  }

  .password-config .password-options .password-option input:checked ~ .checkmark {
    background-color: var(--main-color-primary);
    border: 2px solid var(--main-color-primary);
  }

  .password-config .password-options .password-option input:checked ~ .checkmark:after {
    display: block;
  }

  .password-config .password-generate-button button {
    width: 100%;
    height: 50px;
    font-size: 1.2rem;
    border: 2px solid var(--main-color-primary);
    color: var(--background-color-primary);
    background-color: var(--main-color-primary);
    cursor: pointer;
  }

  .password-config .password-generate-button button:hover {
    color: var(--main-color-primary);
    background-color: var(--background-color-secondary);
  }
</style>
