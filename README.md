# CodeAlpha_ImageGallary
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Responsive Image Gallery</title>

  <style>
    *{
      margin:0;
      padding:0;
      box-sizing:border-box;
      font-family: Arial, sans-serif;
    }

    body{
      background:#f4f4f4;
      padding:20px;
    }

    h1{
      text-align:center;
      margin-bottom:20px;
      color:#333;
    }

    /* FILTER BUTTONS */
    .filters{
      text-align:center;
      margin-bottom:25px;
    }

    .filters button{
      padding:10px 18px;
      margin:5px;
      border:none;
      background:#333;
      color:white;
      border-radius:5px;
      cursor:pointer;
      transition:0.3s;
    }

    .filters button:hover,
    .filters button.active{
      background:#ff6600;
    }

    /* GALLERY */
    .gallery{
      display:grid;
      grid-template-columns:repeat(auto-fit, minmax(250px,1fr));
      gap:18px;
    }

    .gallery-item{
      position:relative;
      overflow:hidden;
      border-radius:10px;
      cursor:pointer;
    }

    .gallery img{
      width:100%;
      height:250px;
      object-fit:cover;
      transition:transform 0.4s ease, filter 0.4s ease;
      display:block;
    }

    /* HOVER EFFECT */
    .gallery-item:hover img{
      transform:scale(1.1);
      filter:brightness(70%);
    }

    .overlay{
      position:absolute;
      bottom:0;
      left:0;
      width:100%;
      padding:15px;
      background:rgba(0,0,0,0.6);
      color:white;
      transform:translateY(100%);
      transition:0.4s ease;
      text-align:center;
    }

    .gallery-item:hover .overlay{
      transform:translateY(0);
    }

    /* LIGHTBOX */
    .lightbox{
      display:none;
      position:fixed;
      z-index:1000;
      top:0;
      left:0;
      width:100%;
      height:100%;
      background:rgba(0,0,0,0.9);
      justify-content:center;
      align-items:center;
      flex-direction:column;
    }

    .lightbox img{
      max-width:90%;
      max-height:80%;
      border-radius:10px;
      animation:zoom 0.3s ease;
    }

    @keyframes zoom{
      from{
        transform:scale(0.7);
      }
      to{
        transform:scale(1);
      }
    }

    .close{
      position:absolute;
      top:20px;
      right:30px;
      color:white;
      font-size:40px;
      cursor:pointer;
    }

    .nav-btn{
      position:absolute;
      top:50%;
      transform:translateY(-50%);
      background:rgba(255,255,255,0.3);
      color:white;
      border:none;
      padding:15px 20px;
      font-size:25px;
      cursor:pointer;
      border-radius:50%;
      transition:0.3s;
    }

    .nav-btn:hover{
      background:#ff6600;
    }

    .prev{
      left:30px;
    }

    .next{
      right:30px;
    }

    /* RESPONSIVE */
    @media(max-width:768px){
      .gallery img{
        height:200px;
      }

      .nav-btn{
        padding:10px 14px;
        font-size:20px;
      }
    }

    @media(max-width:500px){
      h1{
        font-size:24px;
      }

      .gallery img{
        height:180px;
      }
    }
  </style>
</head>

<body>

  <h1>Responsive Image Gallery</h1>

  <!-- FILTER BUTTONS -->
  <div class="filters">
    <button class="active" onclick="filterImages('all')">All</button>
    <button onclick="filterImages('nature')">Nature</button>
    <button onclick="filterImages('city')">City</button>
    <button onclick="filterImages('animals')">Animals</button>
  </div>

  <!-- GALLERY -->
  <div class="gallery">

    <div class="gallery-item nature">
      <img src="https://picsum.photos/id/1015/600/400" alt="">
      <div class="overlay">Nature Image</div>
    </div>

    <div class="gallery-item city">
      <img src="https://picsum.photos/id/1011/600/400" alt="">
      <div class="overlay">City Image</div>
    </div>

    <div class="gallery-item animals">
      <img src="https://picsum.photos/id/1025/600/400" alt="">
      <div class="overlay">Animal Image</div>
    </div>

    <div class="gallery-item nature">
      <img src="https://picsum.photos/id/1039/600/400" alt="">
      <div class="overlay">Nature Image</div>
    </div>

    <div class="gallery-item city">
      <img src="https://picsum.photos/id/1043/600/400" alt="">
      <div class="overlay">City Image</div>
    </div>

    <div class="gallery-item animals">
      <img src="https://picsum.photos/id/237/600/400" alt="">
      <div class="overlay">Animal Image</div>
    </div>

  </div>

  <!-- LIGHTBOX -->
  <div class="lightbox" id="lightbox">

    <span class="close">&times;</span>

    <button class="nav-btn prev">&#10094;</button>

    <img id="lightbox-img" src="" alt="">

    <button class="nav-btn next">&#10095;</button>

  </div>

  <script>

    const galleryImages = document.querySelectorAll(".gallery img");
    const lightbox = document.getElementById("lightbox");
    const lightboxImg = document.getElementById("lightbox-img");
    const closeBtn = document.querySelector(".close");
    const nextBtn = document.querySelector(".next");
    const prevBtn = document.querySelector(".prev");

    let currentIndex = 0;

    // OPEN LIGHTBOX
    galleryImages.forEach((img, index) => {
      img.addEventListener("click", () => {
        currentIndex = index;
        showImage();
        lightbox.style.display = "flex";
      });
    });

    // SHOW IMAGE
    function showImage(){
      lightboxImg.src = galleryImages[currentIndex].src;
    }

    // NEXT IMAGE
    nextBtn.addEventListener("click", () => {
      currentIndex = (currentIndex + 1) % galleryImages.length;
      showImage();
    });

    // PREVIOUS IMAGE
    prevBtn.addEventListener("click", () => {
      currentIndex =
        (currentIndex - 1 + galleryImages.length) % galleryImages.length;
      showImage();
    });

    // CLOSE LIGHTBOX
    closeBtn.addEventListener("click", () => {
      lightbox.style.display = "none";
    });

    // CLOSE WHEN CLICK OUTSIDE IMAGE
    lightbox.addEventListener("click", (e) => {
      if(e.target !== lightboxImg &&
         e.target !== nextBtn &&
         e.target !== prevBtn){
        lightbox.style.display = "none";
      }
    });

    // FILTER FUNCTION
    function filterImages(category){

      const items = document.querySelectorAll(".gallery-item");
      const buttons = document.querySelectorAll(".filters button");

      buttons.forEach(btn => btn.classList.remove("active"));

      event.target.classList.add("active");

      items.forEach(item => {

        if(category === "all"){
          item.style.display = "block";
        }
        else if(item.classList.contains(category)){
          item.style.display = "block";
        }
        else{
          item.style.display = "none";
        }

      });

    }

  </script>

</body>
</html>
