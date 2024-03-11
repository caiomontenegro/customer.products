<template>
  <the-header>
  </the-header>
  <main>
    <slot />
  </main>
  <footer>
    <div>Developed by <a class="developer" href="https://www.linkedin.com/in/caio-montenegro/" target="_blank">Caio Montenegro</a></div>
  </footer>
</template>

<style lang="scss">

html {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: none;
}

body {
  margin: 0px 20px;
  height: 100vh;
  min-width: 350px;
  background-repeat: no-repeat;
  background: linear-gradient(rgb(218, 33, 88), rgb(68, 15, 167));
  color: white;
}

footer {
  position: absolute;
  bottom: 0;
  width: 90%;
  text-align: center;
}

.developer {
  color: brown;
  font-weight: bolder;
}

@media(min-width: 1000px) {
  body {
    width: 700px;
    max-width: 850px;
  }

  footer {
    width: 51%;
  }
}
</style>