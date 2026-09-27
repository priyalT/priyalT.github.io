---
layout: default
title: Home
---
<div class="desktop-workspace">

  <div class="desktop-grid">
    <button class="desktop-icon" id="icon-pc">
      <img src="{{ '/assets/pc.png' | relative_url }}" alt="Priyal's PC">
      <span>Priyal's PC</span>
    </button>
    <button class="desktop-icon" id="icon-projects">
      <img src="{{ '/assets/projects.png' | relative_url }}" alt="Projects">
      <span>Projects</span>
    </button>
    <button class="desktop-icon" id="icon-cv">
      <img src="{{ '/assets/cv.png' | relative_url }}" alt="CV">
      <span>CV</span>
    </button>
    <button class="desktop-icon" id="icon-recycle">
      <img src="{{ '/assets/recycle_bin.png' | relative_url }}" alt="Recycle Bin">
      <span>Recycle Bin</span>
    </button>
  </div>

  <div class="window active" id="win-pc" style="top: 20px; left: 240px; width: 560px;">
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
            Take a look around! Click the <strong>Projects</strong> folder on the desktop to see my projects, or check out my <strong>CV</strong>. I'd love to hear from you too! Click the <strong>Email</strong> to reach out to me and build cool things together.
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

        <button class="retro-yellow-box" style="padding: 2px 8px; font-size: 11px;" onclick="sendQuickReply('What is your research focus?', 'Right now I am focused on spatial transcriptomics. Previously, I have also worked in cancer biomarker discovery.')">Research</button>
      </div>

    </div>

    <div class="status-bar">
      <p class="status-bar-field">Status: Online</p>
      <p class="status-bar-field">3 quick questions available</p>
    </div>
  </div>

  <div class="window hidden" id="win-projects" style="top: 80px; left: 340px; width: 480px;">
    <div class="title-bar">
      <div class="title-bar-text">📁 Research Projects - C:\Lab\Projects</div>
      <div class="title-bar-controls">
        <button aria-label="Minimize" data-minimize="win-projects"></button>
        <button aria-label="Maximize"></button>
        <button aria-label="Close" data-close="win-projects"></button>
      </div>
    </div>
    <div class="window-body">
      <p>Active research pipelines & plots:</p>
      
      <div style="background: #fff; border: 1px solid #7f9db9; padding: 8px; margin-bottom: 8px;">
        <strong>🔬 01. Spatial Transcriptomics QC</strong>
        <p style="margin: 4px 0 0 0; font-size: 11px; color: #444;">Computational approaches for spatial omics data quality control.</p>
      </div>
      <div style="background: #fff; border: 1px solid #7f9db9; padding: 8px; margin-bottom: 8px;">
        <strong>🧬 02. Cancer Biomarker Discovery</strong>
        <p style="margin: 4px 0 0 0; font-size: 11px; color: #444;">Machine learning models applied to sequencing data.</p>
      </div>
      <div style="background: #fff; border: 1px solid #7f9db9; padding: 8px;">
        <strong>💻 03. Biological Machine Learning</strong>
        <p style="margin: 4px 0 0 0; font-size: 11px; color: #444;">Tree-based and deep learning architectures for biological datasets.</p>
      </div>
    </div>
    <div class="status-bar">
      <p class="status-bar-field">3 item(s)</p>
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
      <p>Education, research experience, and publications.</p>
      <div style="margin-top: 14px;">
        <a class="retro-yellow-box" href="{{ '/cv/' | relative_url }}">Open CV Document ↗</a>
      </div>
    </div>
    <div class="status-bar">
      <p class="status-bar-field">Page 1 of 1</p>
    </div>
  </div>


  <div class="window hidden" id="win-recycle" style="top: 160px; left: 300px; width: 340px;">
    <div class="title-bar">
      <div class="title-bar-text">Recycle Bin</div>
      <div class="title-bar-controls">
        <button aria-label="Minimize" data-minimize="win-recycle"></button>
        <button aria-label="Maximize"></button>
        <button aria-label="Close" data-close="win-recycle"></button>
      </div>
    </div>
    <div class="window-body" style="text-align: center; padding: 20px;">
      <p style="color: #666; font-style: italic;">The Recycle Bin is empty.</p>
    </div>
    <div class="status-bar">
      <p class="status-bar-field">0 items</p>
    </div>
  </div>
</div>


<script>
  (function () {
    let highestZ = 100;
    document.querySelectorAll(".desktop-workspace .window").forEach(function (win) {
      win.addEventListener("mousedown", function () {
        highestZ++;
        win.style.zIndex = highestZ;
        document.querySelectorAll(".desktop-workspace .window").forEach(w => w.classList.remove("active"));
        win.classList.add("active");
      });
    });
    function bindIcon(iconId, windowId) {
      const icon = document.getElementById(iconId);
      const win = document.getElementById(windowId);
      if (!icon || !win) return;
      icon.addEventListener("click", function () {
        win.classList.remove("hidden");
        highestZ++;
        win.style.zIndex = highestZ;
        document.querySelectorAll(".desktop-workspace .window").forEach(w => w.classList.remove("active"));
        win.classList.add("active");
      });
    }
    bindIcon("icon-pc", "win-pc");
    bindIcon("icon-projects", "win-projects");
    bindIcon("icon-cv", "win-cv");
    bindIcon("icon-recycle", "win-recycle");
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
      titleBar.addEventListener("mousedown", function (e) {
        if (e.target.closest(".title-bar-controls")) return;
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
          document.removeEventListener("mousemove", onMouseMove);
          document.removeEventListener("mouseup", onMouseUp);
        }
        document.addEventListener("mousemove", onMouseMove);
        document.addEventListener("mouseup", onMouseUp);
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

    window.sendContactReply = function () {
      const replyHtml = `You can reach me via:<br><br>` +
        `<a href="https://www.linkedin.com/in/priyal-tripathi-baa54424a/" target="_blank" style="color: #0055ea; font-weight: bold; text-decoration: underline;">LinkedIn Profile ↗</a><br>` +
        `<a href="https://github.com/priyalT" target="_blank" style="color: #0055ea; font-weight: bold; text-decoration: underline;">GitHub (@priyalT) ↗</a><br>` +
        `<a href="mailto:priyaltripathi2910@gmail.com" style="color: #0055ea; font-weight: bold; text-decoration: underline;">priyaltripathi2910@gmail.com ↗</a>`;
      
      window.sendQuickReply('I would like to introduce myself.', replyHtml);
    };

    };
  })();
</script>
