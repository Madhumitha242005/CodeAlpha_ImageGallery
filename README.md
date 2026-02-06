<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Professional Image Gallery</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    background: #f5f5f5;
    text-align: center;
    padding: 20px;
}

h1 {
    margin-bottom: 20px;
}

.filters {
    margin-bottom: 20px;
}

.filters button {
    padding: 8px 16px;
    margin: 5px;
    border: none;
    background: #333;
    color: white;
    border-radius: 20px;
    cursor: pointer;
    transition: 0.3s;
}

.filters button.active,
.filters button:hover {
    background: #ff4081;
}

.gallery {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(230px, 1fr));
    gap: 15px;
}

.gallery-item img {
    width: 100%;
    height: 220px;
    object-fit: cover;
    border-radius: 10px;
    cursor: pointer;
    transition: 0.4s;
}

.gallery-item img:hover {
    transform: scale(1.05);
}

.hide {
    display: none;
}

/* Lightbox */
.lightbox {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.9);
    justify-content: center;
    align-items: center;
}

.lightbox img {
    max-width: 85%;
    max-height: 80%;
    border-radius: 10px;
}

.close {
    position: absolute;
    top: 20px;
    right: 30px;
    font-size: 35px;
    color: white;
    cursor: pointer;
}

.nav {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    font-size: 40px;
    background: none;
    border: none;
    color: white;
    cursor: pointer;
}

.prev { left: 40px; }
.next { right: 40px; }

@media(max-width: 600px){
    .gallery-item img {
        height: 160px;
    }
}
</style>
</head>

<body>

<h1>Professional Image Gallery</h1>

<div class="filters">
    <button class="active" data-filter="all">All</button>
    <button data-filter="nature">Nature</button>
    <button data-filter="animals">Animals</button>
    <button data-filter="city">City</button>
    <button data-filter="technology">Technology</button>
</div>

<div class="gallery">

    <!-- NATURE -->
    <div class="gallery-item nature">
        <img src="https://images.unsplash.com/photo-1501785888041-af3ef285b470?w=600&h=400&fit=crop">
    </div>
    <div class="gallery-item nature">
        <img src="https://images.unsplash.com/photo-1500530855697-b586d89ba3ee?w=600&h=400&fit=crop">
    </div>
    <div class="gallery-item nature">
        <img src="https://images.unsplash.com/photo-1470770841072-f978cf4d019e?w=600&h=400&fit=crop">
    </div>
    <div class="gallery-item nature">
        <img src="https://images.unsplash.com/photo-1441974231531-c6227db76b6e?w=600&h=400&fit=crop">
    </div>
    <div class="gallery-item nature">
        <img src="https://images.unsplash.com/photo-1433086966358-54859d0ed716?w=600&h=400&fit=crop">
    </div>

    <!-- ANIMALS -->
    <div class="gallery-item animals"><img src="https://picsum.photos/id/237/600/400"></div>
    <div class="gallery-item animals"><img src="https://picsum.photos/id/433/600/400"></div>
    <div class="gallery-item animals"><img src="https://picsum.photos/id/593/600/400"></div>
    <div class="gallery-item animals"><img src="https://picsum.photos/id/659/600/400"></div>
    <div class="gallery-item animals"><img src="https://picsum.photos/id/718/600/400"></div>

    <!-- CITY -->
    <div class="gallery-item city">
        <img src="https://images.unsplash.com/photo-1467269204594-9661b134dd2b?w=600&h=400&fit=crop">
    </div>
    <div class="gallery-item city">
        <img src="https://images.unsplash.com/photo-1508057198894-247b23fe5ade?w=600&h=400&fit=crop">
    </div>
    <div class="gallery-item city">
        <img src="https://images.unsplash.com/photo-1496568816309-51d7c20e3b21?w=600&h=400&fit=crop">
    </div>
    <div class="gallery-item city">
        <img src="https://images.unsplash.com/photo-1494526585095-c41746248156?w=600&h=400&fit=crop">
    </div>
    <div class="gallery-item city">
        <img src="https://images.unsplash.com/photo-1500530855697-b586d89ba3ee?w=600&h=400&fit=crop">
    </div>

    <!-- TECHNOLOGY -->
    <div class="gallery-item technology"><img src="https://picsum.photos/id/180/600/400"></div>
    <div class="gallery-item technology"><img src="https://picsum.photos/id/201/600/400"></div>
    <div class="gallery-item technology"><img src="https://picsum.photos/id/250/600/400"></div>
    <div class="gallery-item technology"><img src="https://picsum.photos/id/365/600/400"></div>
    <div class="gallery-item technology"><img src="https://picsum.photos/id/403/600/400"></div>

</div>

<div class="lightbox" id="lightbox">
    <span class="close">&times;</span>
    <button class="nav prev">&#10094;</button>
    <img id="lightbox-img">
    <button class="nav next">&#10095;</button>
</div>

<script>
document.addEventListener("DOMContentLoaded", () => {

    const filterButtons = document.querySelectorAll(".filters button");
    const items = document.querySelectorAll(".gallery-item");
    const images = document.querySelectorAll(".gallery-item img");

    const lightbox = document.getElementById("lightbox");
    const lightboxImg = document.getElementById("lightbox-img");
    const closeBtn = document.querySelector(".close");
    const nextBtn = document.querySelector(".next");
    const prevBtn = document.querySelector(".prev");

    let currentIndex = 0;
    let visibleItems = [...items];

    filterButtons.forEach(button => {
        button.addEventListener("click", () => {

            document.querySelector(".filters .active").classList.remove("active");
            button.classList.add("active");

            const filter = button.dataset.filter;
            visibleItems = [];

            items.forEach(item => {
                if (filter === "all" || item.classList.contains(filter)) {
                    item.classList.remove("hide");
                    visibleItems.push(item);
                } else {
                    item.classList.add("hide");
                }
            });
        });
    });

    images.forEach(img => {
        img.addEventListener("click", () => {
            currentIndex = visibleItems.findIndex(item =>
                item.contains(img)
            );
            lightbox.style.display = "flex";
            lightboxImg.src = img.src;
        });
    });

    closeBtn.onclick = () => lightbox.style.display = "none";
    lightbox.onclick = (e) => {
        if (e.target === lightbox) lightbox.style.display = "none";
    };

    nextBtn.onclick = () => {
        currentIndex = (currentIndex + 1) % visibleItems.length;
        lightboxImg.src =
            visibleItems[currentIndex].querySelector("img").src;
    };

    prevBtn.onclick = () => {
        currentIndex =
            (currentIndex - 1 + visibleItems.length) % visibleItems.length;
        lightboxImg.src =
            visibleItems[currentIndex].querySelector("img").src;
    };

});
</script>

</body>
</html>