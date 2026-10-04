---
layout: default
title: Home
---
<div class="desktop-workspace">

  <div class="desktop-grid">
    <button class="desktop-icon" id="icon-pc">
      <img src="{{ '/assets/images/pc.png' | relative_url }}" alt="Priyal's PC">
      <span>Priyal's PC</span>
    </button>
    <button class="desktop-icon" id="icon-research">
      <img src="{{ '/assets/images/research.png' | relative_url }}" alt="Researchwork">
      <span>Research</span>
    </button>
    <button class="desktop-icon" id="icon-projects">
      <img src="{{ '/assets/images/projects.png' | relative_url }}" alt="Projects">
      <span>Blog posts</span>
    </button>
    <button class="desktop-icon" id="icon-cv">
      <img src="{{ '/assets/images/cv.png' | relative_url }}" alt="CV">
      <span>CV</span>
    </button>
  </div>

  <button class="desktop-icon" id="icon-recycle">
    <img src="{{ '/assets/images/recycle_bin.png' | relative_url }}" alt="Recycle Bin">
    <span>Recycle Bin</span>
  </button>

  <div class="desktop-mascot">
    <div class="mascot-bubble">
      Click around to explore! 🧬
    </div>
    <img src="{{ '/assets/images/character.png' | relative_url }}" alt="Mascot Character" class="mascot-img">
  </div>

  <div class="window hidden" id="win-pc" style="top: 20px; left: 240px; width: 560px;">
    <div class="title-bar">
      <div class="title-bar-text">Priyal - Windows Messenger</div>
      <div class="title-bar-controls">
        <button aria-label="Minimize" data-minimize="win-pc"></button>
        <button aria-label="Maximize"></button>
        <button aria-label="Close" data-close="win-pc"></button>
      </div>
    </div>
    <div class="window-body" style="padding: 10px; display: flex; flex-direction: column; gap: 8px;">
      <div style="display: flex; align-items: center; gap: 10px; padding: 6px 8px; background: #f0f0f0; border: 1px solid #d4d0c8; border-radius: 3px;">
        <div style="position: relative; width: 36px; height: 36px; background: #e0e7ff; border: 1px solid #818cf8; border-radius: 4px; display: flex; align-items: center; justify-content: center; font-size: 20px;">
          👩‍💻
          <span style="position: absolute; bottom: -2px; right: -2px; width: 10px; height: 10px; background: #22c55e; border: 2px solid white; border-radius: 50%;"></span>
        </div>
        <div style="flex: 1; min-width: 0;">
          <div style="font-weight: bold; font-size: 12px; color: #1e3a8a;">Priyal Tripathi (Online)</div>
          <div style="font-size: 11px; color: #666; font-style: italic; overflow: hidden; text-overflow: ellipsis; white-space: nowrap;">
            Decoding biology one gene at a time :3
          </div>
        </div>
      </div>
      <div id="chat-messages" style="background: #ffffff; border: 2px solid #7f9db9; padding: 12px; height: 260px; overflow-y: auto; display: flex; flex-direction: column; gap: 10px; font-family: 'Tahoma', sans-serif; font-size: 12px;">
        <div style="color: #888; font-size: 10px; text-align: center; border-bottom: 1px dashed #e5e5e5; padding-bottom: 4px;">
          Conversation started just now
        </div>

        <div>
          <span style="color: #0b4cb4; font-weight: bold;">Priyal:</span>
          <div style="margin: 2px 0 0 6px; background: #eef5ff; padding: 6px 10px; border-radius: 4px; border: 1px solid #d0e2ff; display: inline-block;">
            Hi! Thanks for coming online!
          </div>
        </div>

        <div>
          <span style="color: #0b4cb4; font-weight: bold;">Priyal:</span>
          <div style="margin: 2px 0 0 6px; background: #eef5ff; padding: 6px 10px; border-radius: 4px; border: 1px solid #d0e2ff; display: inline-block;">
            I'm so glad you're here. I'm Priyal! Welcome to my lab.
          </div>
        </div>

        <div>
          <span style="color: #0b4cb4; font-weight: bold;">Priyal:</span>
          <div style="margin: 2px 0 0 6px; background: #eef5ff; padding: 6px 10px; border-radius: 4px; border: 1px solid #d0e2ff; display: inline-block;">
            I'm just a really curious person who is currently working in <strong>bioinformatics</strong>, <strong>spatial omics</strong>, and <strong>machine learning</strong>.
          </div>
        </div>

        <div>
          <span style="color: #0b4cb4; font-weight: bold;">Priyal:</span>
          <div style="margin: 2px 0 0 6px; background: #eef5ff; padding: 6px 10px; border-radius: 4px; border: 1px solid #d0e2ff; display: inline-block;">
            I spend my days developing computational pipelines, and drinking as much cold coffee as I possibly can xD 
          </div>
        </div>

        <div>
          <span style="color: #0b4cb4; font-weight: bold;">Priyal:</span>
          <div style="margin: 2px 0 0 6px; background: #eef5ff; padding: 6px 10px; border-radius: 4px; border: 1px solid #d0e2ff; display: inline-block;">
            Take a look around! Click the <strong>Projects</strong> folder on the desktop to see my projects, or check out my <a href="https://drive.google.com/file/d/14ZViGZw54LSQhn7V4355_lZlt7oPhui6/view?usp=sharing" target="_blank" rel="noopener noreferrer" style="color: #0b4cb4; font-weight: bold; text-decoration: underline;">CV ↗</a>. I'd love to hear from you too! Click the <strong>Email</strong> to reach out to me and build cool things together.
          </div>
        </div>

        <div>
          <span style="color: #0b4cb4; font-weight: bold;">Priyal:</span>
          <div style="margin: 2px 0 0 6px; background: #eef5ff; padding: 6px 10px; border-radius: 4px; border: 1px solid #d0e2ff; display: inline-block;">
            PS: I also have a dog named Oscar whom I absolutely adore <3
          </div>
        </div>

      </div>

      <div style="display: flex; gap: 4px; flex-wrap: wrap; margin-top: 2px;">
        <button class="retro-yellow-box" style="padding: 2px 8px; font-size: 11px;" onclick="sendQuickReply('What tools have you used?', 'I primarily work in Python (PyTorch, Scanpy, scikit-learn), R (Seurat, Bioconductor), Nextflow, and Linux! I also have experience working with HPC clusters.')">Stack</button>

        <button class="retro-yellow-box" style="padding: 2px 8px; font-size: 11px;" onclick="sendContactReply()">Contact</button>

        <button class="retro-yellow-box" style="padding: 2px 8px; font-size: 11px;" onclick="sendResearchReply()">Research</button>
      </div>

    </div>

    <div class="status-bar">
      <p class="status-bar-field">Status: Online</p>
      <p class="status-bar-field">3 quick questions available</p>
    </div>
  </div>

  <div class="window hidden" id="win-research" style="top: 80px; left: 340px; width: 480px;">
    <div class="title-bar">
      <div class="title-bar-text">Lab Documents - C:\Lab Results</div>
      <div class="title-bar-controls">
        <button aria-label="Minimize" data-minimize="win-research"></button>
        <button aria-label="Maximize"></button>
        <button aria-label="Close" data-close="win-research"></button>
      </div>
    </div>
    <div class="window-body">
      <p style="margin: 0 0 8px 0; font-weight: bold;">Research work</p>
      
      {% for post in site.research_posts limit: 3 %}
        <div style="background: #fff; border: 1px solid #7f9db9; padding: 8px; margin-bottom: 8px;">
          <strong> <a href="{{ post.url | relative_url }}"> {{ post.title }} </a></strong>
          <p style="margin: 4px 0 0 0; font-size: 11px; color: #444;">{{ post.description }}</p>
        </div>
      {% endfor %}
      <div style="margin-top: 12px; display: flex; gap: 8px;">
        <a class="retro-yellow-box" href="{{ '/research/' | relative_url }}">All Research ↗</a>
      </div>
    </div>
    <div class="status-bar">
      <p class="status-bar-field">Status: Ready</p>
    </div>
  </div>

  <div class="window hidden" id="win-blog" style="top: 80px; left: 340px; width: 480px;">
    <div class="title-bar">
      <div class="title-bar-text">Lab Explorer - C:\Lab</div>
      <div class="title-bar-controls">
        <button aria-label="Minimize" data-minimize="win-blog"></button>
        <button aria-label="Maximize"></button>
        <button aria-label="Close" data-close="win-blog"></button>
      </div>
    </div>
    <div class="window-body">
      <p style="margin: 0 0 8px 0; font-weight: bold;">Recent Notes &amp; Writing</p>
      
      {% for post in site.posts limit: 1 %}
        <div style="background: #fff; border: 1px solid #7f9db9; padding: 8px; margin-bottom: 8px;">
          <strong> <a href="{{ post.url | relative_url }}"> {{ post.title }} </a></strong>
          <p style="margin: 4px 0 0 0; font-size: 11px; color: #444;">{{ post.description }}</p>
        </div>
      {% else %}
        <div style="background: #fff; border: 1px solid #7f9db9; padding: 8px; margin-bottom: 8px; color: #64748b; font-style: italic;">
          No posts published yet. Stay tuned!
        </div>
      {% endfor %}
      <div style="margin-top: 12px; display: flex; gap: 8px;">
        <a class="retro-yellow-box" href="{{ '/blog/' | relative_url }}">All Blog Posts ↗</a>
        <a class="retro-yellow-box" href="{{ '/research/' | relative_url }}">All Research ↗</a>
      </div>
    </div>
    <div class="status-bar">
      <p class="status-bar-field">Status: Ready</p>
    </div>
  </div>

  
  <div class="window hidden" id="win-cv" style="top: 140px; left: 380px; width: 420px;">
    <div class="title-bar">
      <div class="title-bar-text">Curriculum Vitae - Document Viewer</div>
      <div class="title-bar-controls">
        <button aria-label="Minimize" data-minimize="win-cv"></button>
        <button aria-label="Maximize"></button>
        <button aria-label="Close" data-close="win-cv"></button>
      </div>
    </div>
    <div class="window-body">
      <p><strong>Priyal Tripathi — Resume / CV</strong></p>
      <p>Education, research experience, and more.</p>
      <div style="margin-top: 14px;">
        <a class="retro-yellow-box" href="https://drive.google.com/file/d/14ZViGZw54LSQhn7V4355_lZlt7oPhui6/view?usp=sharing" target="_blank" rel="noopener noreferrer">Open CV↗</a>
      </div>
    </div>
    <div class="status-bar">
      <p class="status-bar-field">Page 1 of 1</p>
    </div>
  </div>


  <div class="window hidden" id="win-recycle" style="top: 40px; left: 200px; width: 640px; max-width: 92vw;">
    <div class="title-bar">
      <div class="title-bar-text">Recycle Bin</div>
      <div class="title-bar-controls">
        <button aria-label="Minimize" data-minimize="win-recycle"></button>
        <button aria-label="Maximize"></button>
        <button aria-label="Close" data-close="win-recycle"></button>
      </div>
    </div>

    <div style="background: #ece9d8; border-bottom: 1px solid #d4d0c8; padding: 4px 8px; font-family: 'Tahoma', sans-serif; font-size: 11px; display: flex; align-items: center; gap: 8px;">
      <span style="color: #666;">Address:</span>
      <div style="background: #fff; border: 1px solid #7f9db9; padding: 2px 6px; flex: 1; border-radius: 2px;">
        📁 C:\Recycle Bin\Photos
      </div>
    </div>

    <div class="window-body" style="padding: 10px;">
      <p style="margin: 0 0 10px 0; font-size: 12px; color: #475569;">
        Things I couldn't bring myself to permanently delete :3
      </p>

      <div class="recycle-gallery">
        
        <div class="photo-card">
          <img src="{{ '/assets/images/recycle_bin/cabin_view.png' | relative_url }}" alt="Photo 1">
          <span>cabin_view.jpg</span>
        </div>

        <div class="photo-card">
          <img src="{{ '/assets/images/recycle_bin/oscar.jpg' | relative_url }}" alt="Photo 2">
          <span>oscar_book.jpg</span>
        </div>

        <div class="photo-card">
          <img src="{{ '/assets/images/recycle_bin/dumplings.jpg' | relative_url }}" alt="Photo 3">
          <span>dumplings.jpg</span>
        </div>

        <div class="photo-card">
          <img src="{{ '/assets/images/recycle_bin/oscu.jpg' | relative_url }}" alt="Photo 4">
          <span>oscar.jpg</span>
        </div>

      </div>
    </div>

    <div class="status-bar">
      <p class="status-bar-field">4 object(s)</p>
      <p class="status-bar-field">Disk space: 14.8 MB</p>
    </div>
  </div>
</div>


<script>
  (function () {
    let highestZ = 100;
    document.querySelectorAll(".desktop-workspace .window").forEach(function (win) {
      win.addEventListener("pointerdown", function () {
        highestZ++;
        win.style.zIndex = highestZ;
        document.querySelectorAll(".desktop-workspace .window").forEach(w => w.classList.remove("active"));
        win.classList.add("active");
      });
    });
    // Keep a window inside the visible desktop area (e.g. on narrower screens)
    function clampWindow(win) {
      if (window.matchMedia("(max-width: 767px)").matches) return;
      const ws = win.offsetParent;
      if (!ws) return;
      const maxLeft = Math.max(0, ws.clientWidth - win.offsetWidth);
      if (win.offsetLeft > maxLeft) win.style.left = maxLeft + "px";
      if (win.offsetLeft < 0) win.style.left = "0px";
      if (win.offsetTop < 0) win.style.top = "0px";
    }
    window.addEventListener("resize", function () {
      document.querySelectorAll(".desktop-workspace .window:not(.hidden)").forEach(clampWindow);
    });
    function bindIcon(iconId, windowId) {
      const icon = document.getElementById(iconId);
      const win = document.getElementById(windowId);
      if (!icon || !win) return;
      icon.addEventListener("click", function () {
        win.classList.remove("hidden");
        clampWindow(win);
        highestZ++;
        win.style.zIndex = highestZ;
        document.querySelectorAll(".desktop-workspace .window").forEach(w => w.classList.remove("active"));
        win.classList.add("active");
      });
    }
    bindIcon("icon-pc", "win-pc");
    bindIcon("icon-projects", "win-blog");
    bindIcon("icon-cv", "win-cv");
    bindIcon("icon-recycle", "win-recycle");
    bindIcon("icon-research", "win-research");
    document.querySelectorAll("[data-close]").forEach(function (btn) {
      btn.addEventListener("click", function (e) {
        e.stopPropagation();
        const win = document.getElementById(btn.getAttribute("data-close"));
        if (win) win.classList.add("hidden");
      });
    });
    document.querySelectorAll("[data-minimize]").forEach(function (btn) {
      btn.addEventListener("click", function (e) {
        e.stopPropagation();
        const win = document.getElementById(btn.getAttribute("data-minimize"));
        if (win) win.classList.add("hidden");
      });
    });
    document.querySelectorAll(".desktop-workspace .window").forEach(function (win) {
      const titleBar = win.querySelector(".title-bar");
      if (!titleBar) return;
      let isDragging = false;
      let startX = 0, startY = 0, initialLeft = 0, initialTop = 0;
      titleBar.addEventListener("pointerdown", function (e) {
        if (e.target.closest(".title-bar-controls")) return;
        if (window.matchMedia("(max-width: 767px)").matches) return;
        isDragging = true;
        highestZ++;
        win.style.zIndex = highestZ;
        document.querySelectorAll(".desktop-workspace .window").forEach(w => w.classList.remove("active"));
        win.classList.add("active");
        startX = e.clientX;
        startY = e.clientY;
        initialLeft = win.offsetLeft;
        initialTop = win.offsetTop;
        function onMouseMove(ev) {
          if (!isDragging) return;
          const dx = ev.clientX - startX;
          const dy = ev.clientY - startY;
          win.style.left = (initialLeft + dx) + "px";
          win.style.top = (initialTop + dy) + "px";
        }
        function onMouseUp() {
          isDragging = false;
          document.removeEventListener("pointermove", onMouseMove);
          document.removeEventListener("pointerup", onMouseUp);
        }
        document.addEventListener("pointermove", onMouseMove);
        document.addEventListener("pointerup", onMouseUp);
      });
    });

    window.sendQuickReply = function (question, answer) {
      const box = document.getElementById("chat-messages");
      if (!box) return;

      const qDiv = document.createElement("div");
      qDiv.style.textAlign = "right";
      qDiv.innerHTML = '<span style="color: #666; font-weight: bold;">You:  </span><div style="margin: 2px 0 0 auto; background: #e2e8f0; padding: 6px 10px; border-radius: 4px; border: 1px solid #cbd5e1; display: inline-block; text-align: left;">' + question + '</div>';
      box.appendChild(qDiv);

      setTimeout(function () {
        const aDiv = document.createElement("div");
        aDiv.innerHTML = '<span style="color: #0b4cb4; font-weight: bold;">Priyal:</span><div style="margin: 2px 0 0 6px; background: #eef5ff; padding: 6px 10px; border-radius: 4px; border: 1px solid #d0e2ff; display: inline-block;">' + answer + '</div>';
        box.appendChild(aDiv);
        box.scrollTop = box.scrollHeight;
      }, 350);

      box.scrollTop = box.scrollHeight;
    };

    window.sendContactReply = function () {
      const replyHtml = `You can reach me via:<br><br>` +
        `<a href="https://www.linkedin.com/in/priyal-tripathi-baa54424a/" target="_blank" style="color: #0055ea; font-weight: bold; text-decoration: underline;">LinkedIn Profile ↗</a><br>` +
        `<a href="https://github.com/priyalT" target="_blank" style="color: #0055ea; font-weight: bold; text-decoration: underline;">GitHub (@priyalT) ↗</a><br>` +
        `<a href="mailto:priyaltripathi2910@gmail.com" style="color: #0055ea; font-weight: bold; text-decoration: underline;">priyaltripathi2910@gmail.com ↗</a>`;
      
      window.sendQuickReply('I would like to introduce myself.', replyHtml);
    };

    window.sendResearchReply = function () {
      const researchUrl = "{{ '/research/' | relative_url }}";
      const replyHtml = 'Right now I am focused on spatial transcriptomics. Previously, I have also worked in cancer biomarker discovery and machine learning. You can explore all my publications, conference presentations, and research projects on the <a href="' + researchUrl + '" style="color: #0055ea; font-weight: bold; text-decoration: underline;">Research ↗</a> page!';
      window.sendQuickReply('What is your research focus?', replyHtml);
    };
  })();
</script>
