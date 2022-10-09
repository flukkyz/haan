<template>
  <v-app dark>
    <v-main>
      <Nuxt />
    </v-main>
    <client-only>
      <snackbar />
    </client-only>
  </v-app>
</template>

<script>
export default {
}
</script>

<style lang="scss">
  .text-field-right{
    .v-text-field__slot{
    input{
      text-align: right !important;
    }
    }
  }
  .pointer {
    cursor: pointer;
  }
  .move{
    cursor: move;
  }
  .on-center{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
  }
  .bg-blur{
    -webkit-backdrop-filter: blur(5px);
    backdrop-filter: blur(5px);
    background-color: #FFFFFF44;
  }
  .w-100{
    width: 100%;
  }
  .w-90{
    width: 90%;
  }
  .w-80{
    width: 80%;
  }
  .w-75{
    width: 75%;
  }
  .w-70{
    width: 70%;
  }
  .w-60{
    width: 60%;
  }
  .w-50{
    width: 50%;
  }
  .w-40{
    width: 40%;
  }
  .w-30{
    width: 30%;
  }
  .w-25{
    width: 25%;
  }
  .w-20{
    width: 20%;
  }
  .w-10{
    width: 10%;
  }
  .h-100{
    height: 100%;
  }
  .h-75{
    height: 75%;
  }
  .h-50{
    height: 50%;
  }
  .h-25{
    height: 25%;
  }
  a:hover,
  a:visited,
  a:link,
  a:active {
      text-decoration: none;
  }
  .top-items{
    align-items: flex-start;
  }
  .center-items{
    align-items: center;
  }
  .bottom-items{
    align-items: flex-end;
  }
  .small{
    font-size: 0.7rem !important;
  }
  .very-small{
    font-size: 0.6rem !important;
  }
  .translucent{
    background-color: rgba(255, 255, 255, 0.1);
  }
  .no-bullets {
    list-style-type: none;
  }
  .border-bold {
    border-top: 2px solid #ccc !important;
    border-bottom: 2px solid #ccc !important;
    border-left: 2px solid #ccc !important;
    border-right: 2px solid #ccc !important;
  }
  .border-bold-x {
    border-left: 2px solid #ccc !important;
    border-right: 2px solid #ccc !important;
  }
  .border-bold-y {
    border-top: 2px solid #ccc !important;
    border-bottom: 2px solid #ccc !important;
  }
  .border-bold-right {
    border-right: 2px solid #ccc !important;
  }
  .border-bold-left {
    border-left: 2px solid #ccc !important;
  }
  .border-bold-top {
    border-top: 2px solid #ccc !important;
  }
  .border-bold-bottom {
    border-bottom: 2px solid #ccc !important;
  }
  .border-normal {
    border-top: 1px solid #ccc !important;
    border-bottom: 1px solid #ccc !important;
    border-left: 1px solid #ccc !important;
    border-right: 1px solid #ccc !important;
  }
  .border-normal-x {
    border-left: 1px solid #ccc !important;
    border-right: 1px solid #ccc !important;
  }
  .border-normal-y {
    border-top: 1px solid #ccc !important;
    border-bottom: 1px solid #ccc !important;
  }
  .border-normal-right {
    border-right: 1px solid #ccc !important;
  }
  .border-normal-left {
    border-left: 1px solid #ccc !important;
  }
  .border-normal-top {
    border-top: 1px solid #ccc !important;
  }
  .border-normal-bottom {
    border-bottom: 1px solid #ccc !important;
  }

  :root {
    --ck-image-style-spacing: 1.5em;
  }

  .ck-content {
    & .image-style-side {
      float: right;
      margin-left: var(--ck-image-style-spacing);
      max-width: 50%;
    }

    & .image-style-align-left {
      float: left;
      margin-right: var(--ck-image-style-spacing);
    }

    & .image-style-align-center {
      margin-left: auto;
      margin-right: auto;
    }

    & .image-style-align-right {
      float: right;
      margin-left: var(--ck-image-style-spacing);
    }
  }

  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
  input[type=number] {
    -moz-appearance: textfield;
  }

  .border-light{
    border: solid 2px #eee !important;
  }
  .border-dark{
    border: solid 2px #111 !important;
  }

  .font-Bebas{
    font-family: "Bebas";
  }
  .font-Bignord{
    font-family: "Bignord";
  }
  .font-Helvethaica{
    font-family: "Helvethaica";
  }
  .font-JumPar{
    font-family: "JumPar";
  }
  .font-WiSaKa{
    font-family: "WiSaKa";
  }
  .font-Butterfly{
    font-family: "Butterfly";
  }
  .font-Samson{
    font-family: "Samson";
  }

.relative{
  position: relative;
}
.absolute{
  position: absolute;
}
.shadow{
    box-shadow: 2px 3px 10px #e9e9e9 !important;
}
.text-shadow{
  text-shadow: 1px 1px 1px #999;
}
</style>
