<template>
    <!-- Navbar -->
    <nav class="navbar bg-dark d-flex justify-content-start">
      <div class="mybadge">
        <img src="@/assets/firebase.png" />
        <p class="mb-0">Firebase Crashlytics > Overview</p>
      </div>
    </nav>
</template>

<script>
export default {
    name: "Navbar"
}
</script>

<style lang="scss" scoped>
.navbar {
    // height: 50px;
    // background-color: rgb(3, 155, 229);
    img {
      width: 31px !important;
      height: 31px !important;
      margin: 0px 5px;
    }
    p {
      // color: white;
      font-family: 'Roboto', sans-serif;
      font-size: 14px;
      display: flex;
      flex-direction: column;
      justify-content: center;
    }
    .mybadge {
      display: flex;
      // background: white;
      border-radius: 20px;
      color: #fff;
      font-weight:bold;
      padding: 3px 12px;
    }
}
</style>
