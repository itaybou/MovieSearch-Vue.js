<template>
  <header>
    <router-link to="/"
      ><h1><span>Movie</span>Search</h1></router-link
    >
  </header>
  <main><router-view /></main>
</template>

<style lang="scss">
$phone-br: 640px;

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Fira Sans", sans-serif;

  &::selection {
    background: transparetize(#42b883, 0.5);
  }
}

body {
  background-color: #35495e;
  max-width: 70%;
  margin: auto;

  @media (max-width: $phone-br) {
    max-width: 100%;
  }
}

a {
  text-decoration: none;
}

header {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px 16px;
  background-color: #2c3d4e;
  box-shadow: 0px 0px 6px rgba(0, 0, 0, 0.2);

  h1 {
    font-weight: 800;
    color: #fff;
    font-size: 42px;

    @media (max-width: $phone-br) {
      font-size: 28px;
    }

    span {
      color: #42b883;
    }
  }
}
</style>
