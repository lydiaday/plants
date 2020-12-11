<template>
<div id="app">

  <div class="header">
    <router-link to="/">
      <div class="logo">
        <img src="/ordinary.png">
      </div>
    </router-link>
      <div class="title">
        <h1>Plant Inventory</h1>
      </div>
    <router-link to="/">
      <div class="logo">
        <img src="/ordinary.png">
      </div>
    </router-link>
  </div>
  <div class="addLink">
    <router-link to="/admin">Add or update plant</router-link>
  </div>

  <div class="content">
    <router-view />
  </div>
  <div class="footer">
    <a href="https://github.com/lydiaday/plants.git" target="_blank">Github Link</a>

  </div>
</div>
</template>

<style>
html {
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
  font-size: 16px;
  background: #fff;
  padding: 0px;
  margin: 0px;
}



/* Header */
.header {
  display: flex;
  padding: 10px 100px 0px 100px;
  background-color: #F5F5F5;
  color: #1C454F;
  text-decoration: none;
  justify-content: center;
}


.addLink {
  margin-top: 15px;
  font-family: "SF Pro Text";
  font-size: 20px;
  color: #196F3D;
  text-align: center;
}

.addLink a {
  text-decoration: none;
  color: #196F3D;
}

.logo img {
  height: 100px;
  margin-right: 20px;
  margin-left: 20px;
}

.title {
  margin-top: 5px;
  text-decoration: none;
}

.title h1 {
  font-size: 30px;
}

.content {
  padding: 20px 100px;
  min-height: 500px;
}

/* Footer */
.footer {
  height: 50px;
  padding: 20px 100px 0px 100px;
  background: #e3e3e3;
  font-size: 12px;
}

.footer a {
  color: #000;
}

h1 {
  font-size: 20px;
}

h2 {
  font-size: 14px;
}
</style>
