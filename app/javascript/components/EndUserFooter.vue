<template>
  <footer class="footer bg-dark text-white">
    <div class="container p-3">
      <div class="description mx-auto">
        <div class="mb-3">
          <slot></slot>
        </div>
        <div class="mb-3">
          bilbilに関する最新情報はTwitterでお知らせしております。<br />
          また、お問い合わせはTwitterのDMにて受け付けております。<br />
          bilbilに興味がありましたら、ぜひ活用していただきたいです。
        </div>
        <div class="text-center mb-3">
          <a href="https://twitter.com/icchan_kun" class="btn btn-light">
            <i class="fab fa-twitter me-2"></i>@icchan_kunを見る
          </a>
        </div>
        <div class="text-center">
          <span>@bilbil</span>
        </div>
      </div>
    </div>
  </footer>
</template>

<script></script>

<style scoped>
  .btn-light {
    border-radius: 20px;
  }

  .description {
    width: 65%;
  }
</style>
