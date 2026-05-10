---
## Configure page content in wide column
title: "" # leave blank to exclude
number_categories: 0 # set to zero to exclude
---

<style>
.hero-section {
  max-width: 1100px;
  margin: 0 auto 4rem auto;
  text-align: center;
}

.hero-section h1 {
  font-size: 3.8rem;
  margin-bottom: 0.5rem;
}

.hero-section h4 {
  font-size: 1.2rem;
  line-height: 1.6;
  max-width: 850px;
  margin: 0 auto 2rem auto;
}

.profile-photo {
  width: 340px;
  height: 340px;
  object-fit: cover;
  border-radius: 28px;
  margin: 1.5rem auto 0 auto;
  display: block;
}

.about-text {
  max-width: 850px;
  margin: 0 auto 4rem auto;
  text-align: center;
}

.about-text p {
  text-align: center;
}

.about-text h2 {
  margin-top: 2rem;
}

.more-about {
  max-width: 850px;
  margin: 0 auto 2rem auto;
  text-align: center;
}

.carousel {
  max-width: 1200px;
  margin: 2rem auto 3rem auto;
  overflow: hidden;
}

.carousel-track {
  display: flex;
  gap: 1.5rem;
  width: max-content;
  animation: scrollPhotos 35s linear infinite;
}

.carousel-track:hover {
  animation-play-state: paused;
}

.carousel-track img {
  width: 300px;
  height: 300px;
  object-fit: cover;
  border-radius: 24px;
  flex: 0 0 auto;
}

@keyframes scrollPhotos {
  from {
    transform: translateX(0);
  }
  to {
    transform: translateX(-50%);
  }
}

.page-main,
.page-content,
.article-content,
.content,
main,
article,
.measure,
.measure-wide,
.measure-narrow {
  max-width: 1400px !important;
  width: 100% !important;
  margin-left: auto !important;
  margin-right: auto !important;
}
</style>

<div class="hero-section">

# Hello, I’m Hodan

#### Data Analyst | Economics Graduate | Sustainability & Social Impact

I use data, research, and storytelling to explore issues connected to equity, sustainability, public policy, and community impact.

<img class="profile-photo" src="/img/me2.png" alt="Hodan portrait">

</div>

<div class="about-text">

## About Me

I’m a recent Economics graduate with a specialization in Data Science, passionate about using data to drive positive change. My experience includes data analysis, research, and machine learning, and I enjoy applying these skills to address real-world challenges.

I’m especially interested in social justice, women’s equity, and environmental sustainability. My goal is to advance fair policies, promote renewable energy, and help underrepresented communities access the resources they deserve.

---

## Work Experience

### *Schneider Electric Environmental, Health, and Safety Intern (Aug 2023 – Jan 2025)*

Schneider Electric, recognized as the most sustainable company in 2024, is a global leader in energy management and automation. As an intern in the Environmental, Health, and Safety department, I focused on data analysis to support workplace safety initiatives.

I transformed raw data into actionable insights, helping ensure a safe and sustainable environment for employees while contributing to Schneider Electric’s broader sustainability mission.

### <a href="https://blog.se.com/life-at-schneider-electric/2024/07/25/interns-making-an-impact/" target="_blank" rel="noopener">Featured in Schneider Electric's Intern Impact Campaign</a>

---

## More About Me

My hobbies include reading, running, and drinking a lot of coffee — seriously, you’ll almost never see me without a cup in my hand. I practically live in coffee shops; I love the atmosphere, the aesthetic, and of course a really good cup of coffee.

I’ve been running for a few years, but recently I’ve started taking it more seriously and I’m hoping to attempt my first marathon next year. I love the sense of challenge, discipline, and connection to nature that comes with it.

Outside of that, I enjoy spending time with my husband, being outdoors, and appreciating the little things in life.☺️

</div>

<div class="carousel">
  <div class="carousel-track" id="carouselTrack">
    <img src="/img/me2.png" alt="Hodan photo 1">
    <img src="/img/me4.png" alt="Hodan photo 2">
    <img src="/img/me5.JPG" alt="Hodan photo 3">
    <img src="/img/me6.jpeg" alt="Hodan photo 4">
    <img src="/img/me8.jpeg" alt="Hodan photo 5">
    <img src="/img/me11.jpg" alt="Hodan photo 6">
  </div>
</div>

<script>
  const track = document.getElementById("carouselTrack");
  track.innerHTML += track.innerHTML;
</script>
