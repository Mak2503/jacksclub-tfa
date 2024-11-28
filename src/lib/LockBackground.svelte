<script>
  let { otp = $bindable(), correctOtp = $bindable() } = $props();

  const otpValue = $derived(otp.join(""));
  const isOtpValid = $derived(otpValue === correctOtp);
  const backgroundColor = $derived(
    otpValue.length !== 6
      ? "#e4f5ff"
      : isOtpValid
        ? "#E8FEF3"
        : "var(--danger-bg)"
  );
  const svgColor = $derived(
    otpValue.length !== 6 ? "#0794ff" : isOtpValid ? "#1AAF82" : "#FD4B63"
  );
</script>

<div
  class="lock {otpValue.length === 6 ? 'spread' : ''}"
  style="background-color: {backgroundColor}"
>
  <svg
    height="50"
    viewBox="0 0 354 365"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
  >
    <circle cx="125" cy="240" r="119" stroke={svgColor} stroke-width="12" />
    <line
      x1="199"
      y1="83"
      x2="198"
      y2="143"
      stroke={svgColor}
      stroke-width="12"
    />

    {#if !isOtpValid}
      <line
        x1="50"
        y1="88"
        x2="50"
        y2="148"
        stroke={svgColor}
        stroke-width="12"
      />
      <path
        d="M199 88C199 44.9 165.6 10 124.5 10C83.35 10 50 44.9 50 88"
        stroke={svgColor}
        stroke-width="12"
      />
    {:else}
      <line
        y1="-6"
        x2="60"
        y2="-6"
        transform="matrix(0 1 1 0 354 84)"
        stroke={svgColor}
        stroke-width="12"
      />
      <path
        d="M199 84C199 40.9 232.4 6 273.5 6C314.6 6 348 40.9 348 84"
        stroke={svgColor}
        stroke-width="12"
      />
    {/if}

    <circle cx="125" cy="213" r="10" stroke={svgColor} stroke-width="12" />
    <line
      x1="125"
      y1="227"
      x2="125"
      y2="277"
      stroke={svgColor}
      stroke-width="12"
    />
  </svg>
</div>

<style>
  .lock {
    background-color: #e4f5ff;
    width: 70px;
    height: 70px;
    padding: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 100%;
    -webkit-transition: all 0.5s ease;
    -moz-transition: all 0.5s ease;
    transition: all 0.5s ease;
  }
  .lock svg {
    padding-left: 13px;
    transition: all 0.5s ease-in-out;
  }

  @keyframes spread {
    0% {
      transform: scale(1);
      opacity: 1;
    }
    100% {
      transform: scale(1.71);
      opacity: 0.5;
    }
  }

  .spread {
    animation: spread 0.5s;
  }
</style>
