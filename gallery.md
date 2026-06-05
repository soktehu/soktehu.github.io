---
layout: default
title: Gallery
---

<div class="gallery-intro">
    <h1>Gallery</h1>
    <p>A collection of moments, shapes, and places captured along the way. Click any photo to see it in full view.</p>
</div>

<div class="gallery-grid">
    {% for item in site.data.gallery %}
        <div class="gallery-item" onclick="openLightbox('{{ site.baseurl }}{{ item.image_path }}', '{{ item.title | escape }}', '{{ item.description | escape }}', '{{ item.location | escape }}', '{{ item.year }}')">
            <div class="gallery-img-wrapper">
                <img class="gallery-img" src="{{ site.baseurl }}{{ item.image_path }}" alt="{{ item.title }}">
            </div>
            <div class="gallery-overlay">
                <h3 class="gallery-title-overlay">{{ item.title }}</h3>
                <p class="gallery-desc-overlay">{{ item.description }}</p>
                <div class="gallery-meta-overlay">
                    <span>{{ item.location }}</span>
                    <span>{{ item.year }}</span>
                </div>
            </div>
        </div>
    {% endfor %}
</div>

<!-- Accessible Lightbox Modal -->
<dialog class="lightbox" id="lightbox">
    <div class="lightbox-content">
        <button class="lightbox-close-btn" aria-label="Close image lightbox" onclick="closeLightbox()">×</button>
        <img class="lightbox-image" id="lightbox-img" src="" alt="">
        <div class="lightbox-details">
            <h3 class="lightbox-title" id="lightbox-title"></h3>
            <p class="lightbox-desc" id="lightbox-desc"></p>
            <div class="lightbox-meta">
                <span id="lightbox-location"></span>
                <span id="lightbox-year"></span>
            </div>
        </div>
    </div>
</dialog>

<script>
    const lightbox = document.getElementById('lightbox');
    const lightboxImg = document.getElementById('lightbox-img');
    const lightboxTitle = document.getElementById('lightbox-title');
    const lightboxDesc = document.getElementById('lightbox-desc');
    const lightboxLoc = document.getElementById('lightbox-location');
    const lightboxYear = document.getElementById('lightbox-year');

    function openLightbox(src, title, desc, loc, year) {
        lightboxImg.src = src;
        lightboxImg.alt = title;
        lightboxTitle.textContent = title;
        lightboxDesc.textContent = desc;
        lightboxLoc.textContent = loc ? `📍 ${loc}` : '';
        lightboxYear.textContent = year ? `📅 ${year}` : '';
        
        // Prevent scroll on body when dialog is open
        document.body.style.overflow = 'hidden';
        
        lightbox.showModal();
    }

    function closeLightbox() {
        lightbox.close();
        document.body.style.overflow = '';
    }

    // Close when clicking on backdrop
    lightbox.addEventListener('click', (e) => {
        const rect = lightbox.getBoundingClientRect();
        const isInDialog = (rect.top <= e.clientY && e.clientY <= rect.top + rect.height
          && rect.left <= e.clientX && e.clientX <= rect.left + rect.width);
        if (!isInDialog) {
            closeLightbox();
        }
    });

    // Handle Esc key scroll restore
    lightbox.addEventListener('cancel', () => {
        document.body.style.overflow = '';
    });
</script>
