<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Álbum Virtual - Clasificadas</title>
<link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600&display=swap" rel="stylesheet">
<style>
  :root {
    --bg-color: #0f172a;
    --text-color: #f8fafc;
    --accent: #38bdf8;
    --card-bg: #1e293b;
    --nav-bg: rgba(15, 23, 42, 0.85);
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: 'Outfit', sans-serif;
    background-color: var(--bg-color);
    color: var(--text-color);
    line-height: 1.6;
    overflow-x: hidden;
  }
  
  /* Header & Navigation */
  header {
    text-align: center;
    padding: 3rem 1rem 1rem;
  }
  h1 { 
    font-weight: 600; 
    font-size: 3rem; 
    letter-spacing: -1px;
    background: linear-gradient(135deg, #38bdf8, #818cf8);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    margin-bottom: 1rem;
  }
  .header-desc {
    color: #94a3b8;
    font-size: 1.1rem;
    max-width: 600px;
    margin: 0 auto 2rem;
  }
  
  nav {
    position: sticky;
    top: 0;
    z-index: 100;
    backdrop-filter: blur(12px);
    background: var(--nav-bg);
    padding: 1rem 0;
    border-bottom: 1px solid rgba(255,255,255,0.05);
    box-shadow: 0 4px 20px rgba(0,0,0,0.1);
  }
  .nav-links {
    display: flex;
    justify-content: center;
    gap: 1rem;
    flex-wrap: wrap;
    list-style: none;
    max-width: 1400px;
    margin: 0 auto;
    padding: 0 1rem;
  }
  .nav-links li {
    margin: 0; 
  }
  .nav-links a {
    display: inline-block;
    color: #cbd5e1;
    text-decoration: none;
    font-size: 0.95rem;
    padding: 0.6rem 1.2rem;
    border-radius: 30px;
    transition: all 0.3s ease;
    background: rgba(255,255,255,0.03);
    border: 1px solid rgba(255,255,255,0.05);
  }
  .nav-links a:hover, .nav-links a.active {
    background: var(--accent);
    color: #0f172a;
    font-weight: 600;
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(56, 189, 248, 0.4);
    border-color: transparent;
  }
  
  /* Sections */
  .container { max-width: 1400px; margin: 0 auto; padding: 2rem 2rem 4rem; }
  
  section {
    padding-top: 5rem;
    margin-top: -3rem;
    margin-bottom: 4rem;
    animation: fadeIn 0.8s ease forwards;
    opacity: 0;
    transform: translateY(20px);
  }
  
  .section-header {
    display: flex;
    align-items: center;
    margin-bottom: 2rem;
  }
  
  h2 { 
    font-weight: 400; 
    font-size: 2.2rem; 
    color: #f1f5f9;
  }
  
  .badge {
    background: rgba(56, 189, 248, 0.15);
    color: var(--accent);
    padding: 0.2rem 0.8rem;
    border-radius: 20px;
    font-size: 0.9rem;
    margin-left: 1rem;
    font-weight: 600;
  }

  /* Grid */
  .grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 1.5rem;
  }
  .card {
    background: var(--card-bg);
    border-radius: 16px;
    overflow: hidden;
    position: relative;
    cursor: pointer;
    transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    box-shadow: 0 4px 15px rgba(0,0,0,0.2);
    aspect-ratio: 4/3;
  }
  .card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0; bottom: 0;
    background: linear-gradient(135deg, rgba(255,255,255,0.1) 0%, rgba(255,255,255,0) 100%);
    z-index: 1;
    pointer-events: none;
  }
  .card:hover {
    transform: translateY(-8px) scale(1.03);
    box-shadow: 0 15px 30px rgba(0,0,0,0.4);
    z-index: 2;
  }
  .card img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.6s ease;
  }
  .card:hover img {
    transform: scale(1.1);
  }
  .card-overlay {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    background: linear-gradient(to top, rgba(15, 23, 42, 0.95) 0%, transparent 100%);
    padding: 2.5rem 1.5rem 1rem;
    opacity: 0;
    transition: all 0.3s ease;
    z-index: 2;
    transform: translateY(10px);
  }
  .card:hover .card-overlay {
    opacity: 1;
    transform: translateY(0);
  }
  .image-name {
    font-size: 0.95rem;
    color: #e2e8f0;
    font-weight: 400;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  /* Modal Lightbox */
  .modal {
    display: none;
    position: fixed;
    z-index: 1000;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(15, 23, 42, 0.95);
    backdrop-filter: blur(8px);
    align-items: center;
    justify-content: center;
    opacity: 0;
    transition: opacity 0.3s ease;
  }
  .modal.active {
    display: flex;
    opacity: 1;
  }
  .modal img {
    max-width: 90%;
    max-height: 90vh;
    border-radius: 8px;
    box-shadow: 0 20px 40px rgba(0,0,0,0.5);
    transform: scale(0.95);
    transition: transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    object-fit: contain;
  }
  .modal.active img {
    transform: scale(1);
  }
  .modal-close {
    position: absolute;
    top: 20px;
    right: 30px;
    color: rgba(255,255,255,0.6);
    background: rgba(255,255,255,0.1);
    width: 44px;
    height: 44px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 24px;
    cursor: pointer;
    transition: all 0.3s;
    border: 1px solid rgba(255,255,255,0.1);
  }
  .modal-close:hover { 
    color: white; 
    background: var(--accent);
    border-color: var(--accent);
    transform: rotate(90deg);
  }

  @keyframes fadeIn {
    to { opacity: 1; transform: translateY(0); }
  }
  
  /* Scrollbar */
  ::-webkit-scrollbar { width: 10px; }
  ::-webkit-scrollbar-track { background: #0f172a; }
  ::-webkit-scrollbar-thumb { background: #334155; border-radius: 5px; }
  ::-webkit-scrollbar-thumb:hover { background: #475569; }
</style>
</head>
<body>

  <header>
    <h1>Canon Gallery</h1>
    <p class="header-desc">Una colección virtual de fotografías capturadas y clasificadas meticulosamente, presentadas en un entorno inmersivo de alta calidad.</p>
  </header>

  <nav>
    <ul class="nav-links" id="navContainer">
      <!-- Generated links here -->
    </ul>
  </nav>

  <div class="container" id="galleryContainer">
    <!-- Generated sections here -->
  </div>

  <div class="modal" id="imageModal">
    <div class="modal-close" onclick="closeModal()">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="18" y1="6" x2="6" y2="18"></line><line x1="6" y1="6" x2="18" y2="18"></line></svg>
    </div>
    <img id="modalImg" src="" alt="Ampliada">
  </div>

  <script>
    const basePath = 'file:///D:/canon/clasificadas';
    
    // Categorías con imágenes comprobadas
    const catalog = [
      { folder: 'arquitectura', title: 'Arquitectura', images: ['IMG_6748.JPG'] },
      { folder: 'aves', title: 'Aves', images: ['IMG_6739.JPG','IMG_6758.JPG','IMG_6759.JPG','IMG_6760.JPG','IMG_6761.JPG','IMG_6762.JPG','IMG_6764.JPG','IMG_6766.JPG','IMG_6768.JPG','IMG_6768_230656.JPG','IMG_6769.JPG','IMG_6770.JPG','IMG_6771.JPG','IMG_6772.JPG','IMG_6773.JPG','IMG_6774.JPG','IMG_6775.JPG','IMG_6776.JPG','IMG_6777.JPG','IMG_6778.JPG','IMG_6779.JPG','IMG_6780.JPG','IMG_6781.JPG','IMG_6782.JPG','IMG_6783.JPG','IMG_6784.JPG','IMG_6785.JPG','IMG_6786.JPG','IMG_6787.JPG','IMG_6788.JPG','IMG_6789.JPG','IMG_6790.JPG'] },
      { folder: 'comida', title: 'Comida', images: ['IMG_6747.JPG','IMG_6829.JPG','IMG_6830.JPG','IMG_6831.JPG','IMG_6832.JPG'] },
      { folder: 'mascotas', title: 'Mascotas', images: ['IMG_6791.JPG','IMG_6792.JPG','IMG_6795.JPG','IMG_6795_230957.JPG','IMG_6796.JPG','IMG_6796_231003.JPG','IMG_6799.JPG'] },
      { folder: 'paisajes', title: 'Paisajes', images: ['IMG_6801.JPG'] },
      { folder: 'personas', title: 'Personas', images: ['IMG_6742_230350.JPG','IMG_6743.JPG','IMG_6744.JPG','IMG_6745.JPG','IMG_6746.JPG','IMG_6767.JPG'] },
      { folder: 'plantas', title: 'Plantas', images: ['IMG_6739.JPG','IMG_6741.JPG','IMG_6741_230339.JPG','IMG_6750.JPG','IMG_6760.JPG'] },
      { folder: 'sin_clasificar', title: 'Sin Clasificar', images: ['IMG_6749.JPG','IMG_6751.JPG','IMG_6752.JPG','IMG_6755.JPG','IMG_6756.JPG','IMG_6757.JPG','IMG_6793.JPG','IMG_6794.JPG','IMG_6797.JPG','IMG_6798.JPG','IMG_6802.JPG','IMG_6803.JPG','IMG_6804.JPG','IMG_6805.JPG','IMG_6806.JPG','IMG_6807.JPG','IMG_6808.JPG','IMG_6809.JPG','IMG_6810.JPG','IMG_6811.JPG','IMG_6812.JPG','IMG_6813.JPG','IMG_6814.JPG','IMG_6815.JPG','IMG_6816.JPG','IMG_6817.JPG','IMG_6818.JPG','IMG_6819.JPG','IMG_6820.JPG','IMG_6821.JPG','IMG_6822.JPG','IMG_6823.JPG','IMG_6824.JPG','IMG_6825.JPG','IMG_6826.JPG','IMG_6827.JPG','IMG_6828.JPG','IMG_6833.JPG','IMG_6834.JPG','IMG_6835.JPG','IMG_6836.JPG','IMG_6837.JPG','IMG_6838.JPG','IMG_6839.JPG','IMG_6843.JPG','IMG_6844.JPG'] },
      { folder: 'vehiculos', title: 'Vehículos', images: ['IMG_6842.JPG','IMG_6845.JPG','IMG_6846.JPG'] }
    ];

    const navContainer = document.getElementById('navContainer');
    const galleryContainer = document.getElementById('galleryContainer');

    function initGallery() {
      let delayCounter = 0;
      
      catalog.forEach((section, index) => {
        // Generar Enlace de Navegación
        const li = document.createElement('li');
        const a = document.createElement('a');
        a.href = '#' + section.folder;
        a.textContent = section.title;
        if(index === 0) a.classList.add('active');
        li.appendChild(a);
        navContainer.appendChild(li);

        // Generar Sección
        const sec = document.createElement('section');
        sec.id = section.folder;
        sec.style.animationDelay = `${delayCounter * 0.15}s`;
        delayCounter++;
        
        const headerDiv = document.createElement('div');
        headerDiv.className = 'section-header';
        
        const h2 = document.createElement('h2');
        h2.textContent = section.title;
        
        const badge = document.createElement('span');
        badge.className = 'badge';
        badge.textContent = section.images.length;
        
        headerDiv.appendChild(h2);
        headerDiv.appendChild(badge);
        sec.appendChild(headerDiv);

        // Generar Grid
        const grid = document.createElement('div');
        grid.className = 'grid';

        section.images.forEach(imgName => {
          const card = document.createElement('div');
          card.className = 'card';
          // Usando la ruta base file:/// para poder leer los archivos de D:/canon/clasificadas
          const srcPath = `${basePath}/${section.folder}/${imgName}`;
          card.onclick = () => openModal(srcPath);

          const img = document.createElement('img');
          img.src = srcPath;
          img.alt = imgName;
          img.loading = 'lazy'; // Optimización de carga

          const overlay = document.createElement('div');
          overlay.className = 'card-overlay';
          
          const title = document.createElement('div');
          title.className = 'image-name';
          title.textContent = imgName;
          
          overlay.appendChild(title);
          card.appendChild(img);
          card.appendChild(overlay);
          grid.appendChild(card);
        });

        sec.appendChild(grid);
        galleryContainer.appendChild(sec);
      });
      
      setupScrollSpy();
    }

    // Lógica del Modal (Lightbox)
    const modal = document.getElementById('imageModal');
    const modalImg = document.getElementById('modalImg');

    function openModal(src) {
      modalImg.src = src;
      modal.classList.add('active');
      document.body.style.overflow = 'hidden';
    }

    function closeModal() {
      modal.classList.remove('active');
      document.body.style.overflow = '';
      setTimeout(() => { modalImg.src = ''; }, 300);
    }

    modal.addEventListener('click', (e) => {
      if(e.target === modal || e.target.closest('.modal-close')) {
         closeModal();
      }
    });

    document.addEventListener('keydown', (e) => {
      if(e.key === 'Escape') closeModal();
    });

    // Scroll suave y seguimiento del scroll en el menú
    function setupScrollSpy() {
      const sections = document.querySelectorAll('section');
      const navLinks = document.querySelectorAll('.nav-links a');

      // Scroll suave al hacer click
      navLinks.forEach(anchor => {
        anchor.addEventListener('click', function (e) {
          e.preventDefault();
          const targetId = this.getAttribute('href');
          const targetSection = document.querySelector(targetId);
          if (targetSection) {
            window.scrollTo({
              top: targetSection.offsetTop - 100, // Ajuste por la barra de navegación fija
              behavior: 'smooth'
            });
          }
        });
      });

      // Iluminar enlace activo al hacer scroll
      window.addEventListener('scroll', () => {
        let current = '';
        sections.forEach(section => {
          const sectionTop = section.offsetTop;
          if (pageYOffset >= sectionTop - 150) {
            current = section.getAttribute('id');
          }
        });

        navLinks.forEach(link => {
          link.classList.remove('active');
          if (link.getAttribute('href') === '#' + current) {
            link.classList.add('active');
          }
        });
      });
    }

    // Iniciar galería cuando el DOM esté listo
    document.addEventListener('DOMContentLoaded', initGallery);
  </script>
</body>
</html>
