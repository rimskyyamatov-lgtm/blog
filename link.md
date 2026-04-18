---
layout: page
title: "Link"
---

<style>
.link-container {
  display: grid;
  gap: 16px;
  max-width: 500px;
  margin: 0 auto;
}

.link-card {
  display: flex;
  align-items: center;
  padding: 14px 18px;
  border-radius: 14px;
  text-decoration: none;
  color: #fff;
  font-weight: bold;
  transition: 0.2s;
}

.link-card:hover {
  transform: translateY(-3px);
  opacity: 0.9;
}

/* 個別カラー */
.home { background: #4CAF50; }
.youtube { background: #FF0000; }
.note { background: #41C9B4; }
.github { background: #333; }

.icon {
  font-size: 22px;
  margin-right: 12px;
}
.desc {
  font-size: 12px;
  opacity: 0.8;
}
.icon img {
  width: 24px;
  height: 24px;
  object-fit: contain;
}
</style>

<div class="link-container"> <a class="link-card home" href="http://home.rimsky.f5.si">
      <span class="icon">
        <img src="https://raw.githubusercontent.com/SrinivasaRimskyYamatov/blogimages/main/newtf.png">
      </span>
      <div>
        Home<br>
        <span class="desc">ホームページ</span>
      </div>
    </a>

  <a class="link-card youtube" href="https://www.youtube.com/@%D0%A8%D1%80%D0%B8%D0%BD%D0%B8%D0%B2%D0%B0%D1%81%D0%B0.%D0%A0%D0%B8%D0%BC%D1%81%D0%BA%D0%B8%D0%B9.%D0%AF%D0%BC%D0%B0%D1%82%D0%BE%D0%B2">
    <span class="icon">▶️</span>
    <div>
      YouTube<br>
      <span class="desc">動画投稿（予定）</span>
    </div>
  </a>

  <a class="link-card note" href="https://note.com/rimsky_2672">
    <span class="icon">📝</span>
    <div>
      Note<br>
      <span class="desc">機械語とか</span>
    </div>
  </a>

  <a class="link-card github" href="https://github.com/SrinivasaRimskyYamatov">
    <span class="icon">💻</span>
    <div>
      GitHub<br>
      <span class="desc">コード公開</span>
    </div>
  </a>

</div>
