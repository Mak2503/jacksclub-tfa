<script>
  import { onMount } from "svelte";
  import LockBackground from "./LockBackground.svelte";
  import Button from "./Button.svelte";

  let otp = $state(["", "", "", "", "", ""]);
  let buttonText = $state(`${6} digits left`);

  let correctOtp = $state("123456");

  const getDocument = (id) => document.getElementById(id);

  function handleInput(e, index) {
    const value = e.target.value;

    // Accept only digits
    if (/^\d$/.test(value)) {
      otp[index] = value;

      const currentCursor = getDocument(`cursor-${index}`);
      currentCursor.style.display = "none";

      const currentInput = getDocument(`otp-${index}`);
      currentInput.style.borderColor = "#0794ff";

      // Move to the next input field if it exists
      const nextInput = getDocument(`otp-${index + 1}`);
      if (nextInput) {
        nextInput.focus();
        nextInput.style.borderColor = "#0794ff";
      }

      const nextCursor = getDocument(`cursor-${index + 1}`);
      if (nextCursor) {
        nextCursor.style.display = "block";
      }
    }

    updateButtonText();
  }

  function updateButtonText() {
    const filledDigits = otp.filter((digit) => digit).length;
    buttonText =
      filledDigits < 6
        ? `${6 - filledDigits} digit${filledDigits < 5 ? "s" : ""} left`
        : otp.join("") !== correctOtp
          ? "Wrong code!"
          : "Let's Go";
  }

  function handleKeyDown(e, index) {
    if (e.key === "Backspace" && !otp[index]) {
      const currentInput = getDocument(`otp-${index}`);
      currentInput.style.borderColor = "lightgray";

      const currentCursor = getDocument(`cursor-${index}`);
      currentCursor.style.display = "none";

      // Move to the previous input if backspace is pressed on an empty field
      const previousInput = getDocument(`otp-${index - 1}`);
      if (previousInput) {
        previousInput.focus();
      }

      const nextCursor = getDocument(`cursor-${index - 1}`);
      if (nextCursor) {
        nextCursor.style.display = "block";
      }
    }
  }

  onMount(() => {
    let input = getDocument(`otp-0`);
    let cursor = getDocument(`cursor-0`);
    cursor.style.display = "block";
    input.focus();
  });
</script>

<div class="card">
  <LockBackground bind:otp bind:correctOtp />
  <div style="font-size: 54px; font-weight: 600;">Easy peasy</div>
  <p style="color: #868D91; font-weight: 600;">
    Enter 6-digit code from your two factor authenticator APP.
  </p>
  <div
    class="tfa-code {otp.join('').length === 6 && otp.join('') !== correctOtp
      ? 'shake'
      : ''}"
  >
    {#each otp as digit, i}
      <div
        class="code-input {otp.join('').length === 6 &&
        otp.join('') !== correctOtp
          ? 'wrong-code'
          : 'correct-code'}"
        style={i === 3 && "margin-left: 30px;"}
      >
        <input
          id="otp-{i}"
          type="number"
          bind:value={otp[i]}
          oninput={(e) => handleInput(e, i)}
          onkeydown={(e) => handleKeyDown(e, i)}
          autocomplete="off"
          maxlength="1"
          max="9"
        />
        <div id="cursor-{i}" class="hr-cursor"></div>
      </div>
    {/each}
  </div>
  <Button bind:otp bind:buttonText bind:correctOtp />
</div>

<style>
  /* Chrome, Safari, Edge, Opera */
  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }

  /* Firefox */
  input[type="number"] {
    -moz-appearance: textfield;
    appearance: textfield;
  }

  .card {
    padding: 3rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    border-radius: 20px;
    background-color: #fff;
    box-shadow:
      0 6px 10px 0 rgba(0, 0, 0, 0.2),
      0 6px 10px 0 rgba(0, 0, 0, 0.2);
    text-align: center;
  }

  .tfa-code {
    margin: 40px 0px;
    display: flex;
    justify-content: space-between;
    gap: 20px;
  }

  .code-input {
    position: relative;
  }

  .code-input input {
    font-family: "Urbanist", system-ui, Avenir, Helvetica, Arial, sans-serif;
    max-width: 60px;
    height: 80px;
    text-align: center;
    font-size: 50px;
    color: #0794ff;
    caret-color: transparent;
    border-radius: 10px;
  }

  .correct-code input {
    border: 2px solid lightgray;
  }

  .correct-code input:focus {
    outline: none;
    border: 2px solid #0794ff;
  }

  .wrong-code input {
    color: #fd4b63;
    border: 2px solid #fd4b63 !important; /* To override JS border color */
    background-color: #ffeef0;
  }

  .wrong-code input:focus {
    outline: none;
    border: 2px solid #fd4b63 !important;
    background-color: #ffeef0;
  }

  .hr-cursor {
    position: absolute;
    display: none;
    width: 55%;
    height: 1.5px;
    margin: 0px 15px;
    background-color: #0794ff;
    bottom: 20px;
    -webkit-transform: translateY(-200%);
    -moz-transform: translateY(-200%);
    transform: translateY(-100%);
    -webkit-transition: all 0.5s ease-in-out;
    -moz-transition: all 0.5s ease-in-out;
    transition: all 0.5s ease-in-out;
  }
</style>
