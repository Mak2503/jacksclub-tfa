<script>
  let {
    otp = $bindable(),
    buttonText = $bindable(),
    correctOtp = $bindable(),
  } = $props();
</script>

<button
  disabled={otp.join("").length < 6}
  class={otp.join("").length >= 6 && otp.join("") !== correctOtp ? "shake" : ""}
  style={otp.join("").length < 6
    ? "background-position: left bottom;"
    : otp.join("") !== correctOtp
      ? "background: #FD4B63; color: white;"
      : "color: white;"}
>
  {buttonText}
</button>

<style>
  button {
    background: linear-gradient(to left, #0794ff 50%, #edeef1 50%);
    background-size: 200% 100%;
    background-position: right bottom;
  }
</style>
