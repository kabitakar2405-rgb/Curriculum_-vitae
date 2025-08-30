
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Advanced CV Builder (A4)</title>

  <!-- html2canvas + jsPDF -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

  <style>
    :root{
      --bg1:#0f172a; --bg2:#1e293b; --card:#0b1324; --muted:#94a3b8;
      --primary:#38bdf8; --accent:#facc15; --text:#e5e7eb;
    }
    *{box-sizing:border-box}
    body{
      margin:0; font-family:Inter,system-ui,Arial,sans-serif;
      background:linear-gradient(145deg,var(--bg1),var(--bg2));
      color:var(--text);
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
      padding:24px;
      display:flex; justify-content:center;
    }

    /* Form column (mobile-like narrow card) */
    .form-wrap{
      width:360px;
      max-width:92vw;
      background:rgba(10,14,22,0.85);
      border-radius:14px;
      padding:22px;
      box-shadow:0 12px 30px rgba(2,6,23,0.6);
      border:1px solid rgba(148,163,184,0.06);
      margin-right:24px;
      height: calc(100vh - 48px);
      overflow:auto;
    }
    h1{margin:0 0 10px; font-size:22px; color:var(--accent)}
    p.lead{color:var(--muted); font-size:13px; margin:0 0 18px}

    label{display:block; font-weight:600; color:#cbd5e1; font-size:13px; margin:8px 0 6px}
    input[type="text"], input[type="email"], input[type="tel"],
    input[type="date"], textarea, select{
      width:100%; padding:10px 12px; border-radius:10px; border:1px solid rgba(148,163,184,0.06);
      background:#0d1624; color:var(--text); font-size:14px;
    }
    textarea{min-height:68px; resize:vertical}
    input:focus, textarea:focus, select:focus{outline:none; box-shadow:0 0 0 4px rgba(56,189,248,0.08); border-color:rgba(56,189,248,0.4)}

    .small{font-size:12px; color:var(--muted); margin-top:-6px; margin-bottom:10px}

    .row-two{display:flex; gap:10px}
    .row-two > *{flex:1}

    .actions{display:flex; gap:10px; margin-top:12px}
    .btn{
      flex:1; padding:10px 12px; border-radius:10px; border:0; cursor:pointer; font-weight:700;
      color:#06253b; background:var(--accent);
    }
    .btn.ghost{background:transparent; color:var(--accent); border:1px solid rgba(250,204,21,0.12)}
    .btn.link{background:#0ea5e9; color:#042f3c}

    /* Preview area (A4) */
    .preview-wrap{max-width:800px; width:100%}
    .a4 {
      width:210mm; /* exact A4 width for layout */
      height:auto;
      min-height:297mm;
      padding:16mm;
      background:linear-gradient(180deg,#071026 0%, #071827 100%);
      border-radius:8px;
      box-shadow:0 12px 40px rgba(2,6,23,0.6);
      border:1px solid rgba(148,163,184,0.06);
      overflow:hidden;
      color:var(--text);
      box-sizing:border-box;
      font-size:12.5px; /* moderate readable size */
      line-height:1.25;
    }

    /* layout inside A4 */
    .cv-hero{display:flex; gap:14px; align-items:center; margin-bottom:10px}
    .photo{
      width:84px; height:84px; border-radius:12px; object-fit:cover; border:2px solid var(--primary); background:linear-gradient(135deg,#164eec,#06b6d4); display:flex; align-items:center; justify-content:center; font-weight:800; font-size:22px;
    }
    .name{font-size:20px; font-weight:800; margin:0}
    .tag{color:var(--muted); margin-top:4px; font-size:13px}

    .two-col{display:flex; gap:18px}
    .left-col{width:42%; min-width:160px}
    .right-col{flex:1}

    h3.section-title{color:var(--accent); font-size:13px; margin:6px 0 8px; font-weight:800}

    .kv p{margin:6px 0}
    .kv b{color:#fff}

    table.edu{width:100%; border-collapse:collapse; font-size:12px; margin-top:6px}
    table.edu th, table.edu td{border:1px solid rgba(255,255,255,0.06); padding:6px; text-align:center}
    table.edu th{background:rgba(255,255,255,0.03); color:var(--accent); font-weight:700; font-size:12px}

    .signature{max-height:60px; object-fit:contain; display:block}

    /* print settings - force single A4 */
    @media print{
      body{background:transparent}
      .form-wrap{display:none}
      .a4{box-shadow:none; border-radius:0; padding:12mm; margin:0; width:auto}
      *{ -webkit-print-color-adjust:exact; print-color-adjust:exact; }
    }

    /* small screens - scale preview for view */
    @media (max-width:980px){
      .a4{transform:scale(.7); transform-origin:top left; width:210mm; height:auto}
      .preview-wrap{overflow:auto; max-height:85vh}
    }
  </style>
</head>
<body>

  <!-- LEFT: Form -->
  <div class="form-wrap" role="region" aria-label="CV Form">
    <h1>Advanced CV Builder</h1>
    <p class="lead">Fill all fields. Click <b>Preview CV</b> to render A4 preview. Then <b>Save as PDF</b>.</p>

    <form id="cvForm" onsubmit="return false;">
      <label>Full Name</label>
      <input name="name" placeholder="Ayush Kar" />

      <label>Father's Name</label>
      <input name="father" placeholder="Pradip Kar" />

      <div class="row-two">
        <div>
          <label>Phone</label>
          <input name="phone" placeholder="+91 0000000000" />
        </div>
        <div>
          <label>Email</label>
          <input type="email" name="email" placeholder="name@example.com" />
        </div>
      </div>

      <label>Address</label>
      <textarea name="address" placeholder="Street, Post, PS, District, PIN"></textarea>

      <div class="row-two">
        <div>
          <label>Date of Birth</label>
          <input type="date" name="dob" />
        </div>
        <div>
          <label>Nationality</label>
          <input name="nationality" placeholder="Indian" />
        </div>
      </div>

      <div class="row-two">
        <div>
          <label>Religion</label>
          <input name="religion" placeholder="Hinduism" />
        </div>
        <div>
          <label>Marital Status</label>
          <select name="marital">
            <option value="">-- Select --</option>
            <option>Unmarried</option>
            <option>Married</option>
          </select>
        </div>
      </div>

      <div class="row-two">
        <div>
          <label>Category</label>
          <input name="category" placeholder="General / OBC / SC / ST" />
        </div>
        <div>
          <label>Languages</label>
          <input name="languages" placeholder="Bengali, Hindi, English" />
        </div>
      </div>

      <label>Profile Photo (optional)</label>
      <input type="file" id="profilePhoto" accept="image/*" />

      <label>Signature Photo (optional)</label>
      <input type="file" id="signaturePhoto" accept="image/*" />

      <hr>

      <label>Exam 1 (e.g., 10th)</label>
      <input name="exam1" placeholder="10th PASS" />
      <div class="row-two">
        <input name="board1" placeholder="Board / University (WBHSC)" />
        <input name="year1" placeholder="Year / Status" />
      </div>

      <label>Exam 2 (e.g., 12th)</label>
      <input name="exam2" placeholder="12th PASS" />
      <div class="row-two">
        <input name="board2" placeholder="Board / University" />
        <input name="year2" placeholder="Year / Status" />
      </div>

      <hr>

      <label>Computer Skills (write your own)</label>
      <textarea name="computerSkills" placeholder="Tally Prime, MS Office, Internet, AI, Digital Marketing..."></textarea>

      <label>Other Skills</label>
      <textarea name="otherSkills" placeholder="Drawing, Taekwondo..."></textarea>

      <label>Experience</label>
      <input name="experience" placeholder="Fresher / internship details" />

      <label>Professional Summary</label>
      <textarea name="summary" placeholder="A short professional summary (2-3 lines)"></textarea>

      <label>Declaration</label>
      <textarea name="declaration">I hereby declare that all information furnished above is true to the best of my knowledge.</textarea>

      <div class="row-two">
        <div>
          <label>Place</label>
          <input name="place" placeholder="Kolkata" />
        </div>
        <div>
          <label>Date</label>
          <input type="date" name="date" />
        </div>
      </div>

      <div class="actions">
        <button type="button" class="btn" onclick="generatePreview()">Preview CV</button>
        <button type="button" class="btn link" onclick="downloadPDF()">Save as PDF</button>
      </div>
    </form>
  </div>

  <!-- RIGHT: Preview (A4) -->
  <div class="preview-wrap">
    <div id="cvA4" class="a4" aria-hidden="true" style="display:none;">
      <!-- Rendered by JS -->
    </div>
  </div>

<script>
  // hold images as data URLs
  let profileData = "";
  let signData = "";

  document.getElementById('profilePhoto').addEventListener('change', function(e){
    const f = this.files[0];
    if(!f) { profileData = ""; return; }
    const r = new FileReader();
    r.onload = ev => profileData = ev.target.result;
    r.readAsDataURL(f);
  });

  document.getElementById('signaturePhoto').addEventListener('change', function(e){
    const f = this.files[0];
    if(!f) { signData = ""; return; }
    const r = new FileReader();
    r.onload = ev => signData = ev.target.result;
    r.readAsDataURL(f);
  });

  function safe(v){ return (v||'').toString().trim(); }
  function initials(name){
    if(!name) return "AK";
    const parts = name.trim().split(/\s+/);
    return (parts[0]?.[0]||'') + (parts[1]?.[0]||'');
  }

  function generatePreview(){
    const form = document.getElementById('cvForm');
    const fd = new FormData(form);

    const name = safe(fd.get('name')||'');
    const father = safe(fd.get('father'));
    const phone = safe(fd.get('phone'));
    const email = safe(fd.get('email'));
    const address = safe(fd.get('address'));
    const dob = safe(fd.get('dob'));
    const nationality = safe(fd.get('nationality'));
    const religion = safe(fd.get('religion'));
    const marital = safe(fd.get('marital'));
    const category = safe(fd.get('category'));
    const languages = safe(fd.get('languages'));
    const exam1 = safe(fd.get('exam1')), board1 = safe(fd.get('board1')), year1 = safe(fd.get('year1'));
    const exam2 = safe(fd.get('exam2')), board2 = safe(fd.get('board2')), year2 = safe(fd.get('year2'));
    const computerSkills = safe(fd.get('computerSkills'));
    const otherSkills = safe(fd.get('otherSkills'));
    const experience = safe(fd.get('experience'));
    const summary = safe(fd.get('summary'));
    const declaration = safe(fd.get('declaration'));
    const place = safe(fd.get('place')), date = safe(fd.get('date'));

    // build HTML for A4 preview
    const hero = profileData ? `<img src="${profileData}" class="photo" alt="photo">` : `<div class="photo">${initials(name)}</div>`;

    const eduRows = `
      <tr><td>${exam1}</td><td>${board1}</td><td>${year1}</td></tr>
      <tr><td>${exam2}</td><td>${board2}</td><td>${year2}</td></tr>
    `;

    const previewHTML = `
      <div class="cv-hero">
        ${hero}
        <div>
          <p class="name">${name || 'Your Name'}</p>
          <p class="tag">${summary || 'Fresher — Seeking office/administrative roles'}</p>
        </div>
      </div>

      <div class="two-col">
        <div class="left-col">
          <div>
            <h3 class="section-title">Contact</h3>
            <div class="kv">
              <p><span class="muted">Phone</span><br><b>${phone}</b></p>
              <p><span class="muted">Email</span><br><b>${email}</b></p>
              <p><span class="muted">Address</span><br><b>${address}</b></p>
            </div>
          </div>

          <div>
            <h3 class="section-title">Personal</h3>
            <div class="kv">
              <p><b>Father's Name:</b> ${father}</p>
              <p><b>D.O.B:</b> ${dob}</p>
              <p><b>Nationality:</b> ${nationality}</p>
              <p><b>Religion:</b> ${religion}</p>
              <p><b>Marital Status:</b> ${marital}</p>
              <p><b>Category:</b> ${category}</p>
            </div>
          </div>

          <div>
            <h3 class="section-title">Languages</h3>
            <p><b>${languages}</b></p>
          </div>
        </div>

        <div class="right-col">
          <div>
            <h3 class="section-title">Educational Background</h3>
            <table class="edu">
              <thead><tr><th>Examination</th><th>Board / University</th><th>Year / Status</th></tr></thead>
              <tbody>${eduRows}</tbody>
            </table>
          </div>

          <div style="margin-top:10px">
            <h3 class="section-title">Skills & Computer</h3>
            <p><b>Computer:</b> ${computerSkills || '—'}</p>
            <p><b>Other skills:</b> ${otherSkills || '—'}</p>
          </div>

          <div style="margin-top:10px">
            <h3 class="section-title">Work Experience</h3>
            <p>${experience || 'Fresher — No formal work experience yet.'}</p>
          </div>

          <div style="margin-top:10px">
            <h3 class="section-title">Declaration</h3>
            <p style="color:var(--muted)">${declaration}</p>
            <div style="display:flex; justify-content:space-between; align-items:center; margin-top:10px">
              <div><b>Place:</b> ${place || '—'}<br><b>Date:</b> ${date || '—'}</div>
              <div style="text-align:right">
                ${signData ? `<img src="${signData}" class="signature" alt="sign">` : `<div style="height:42px; color:var(--muted)"><b>Signature</b></div>`}
              </div>
            </div>
          </div>
        </div>
      </div>
    `;

    const cvA4 = document.getElementById('cvA4');
    cvA4.innerHTML = previewHTML;
    cvA4.style.display = 'block';
    cvA4.setAttribute('aria-hidden','false');
    // scroll into view on small screens
    setTimeout(()=>cvA4.scrollIntoView({behavior:'smooth'}),120);
  }

  async function downloadPDF(){
    const cvA4 = document.getElementById('cvA4');
    if(cvA4.style.display === 'none'){
      alert('Please click "Preview CV" first.');
      return;
    }

    // render high-res canvas
    const scale = 2; // increase for better quality
    const canvas = await html2canvas(cvA4, {scale:scale, useCORS:true, backgroundColor: null});
    const imgData = canvas.toDataURL('image/png');

    const { jsPDF } = window.jspdf;
    const pdf = new jsPDF('p','mm','a4');

    const pdfWidth = pdf.internal.pageSize.getWidth(); // 210mm - margins
    // convert canvas px to mm using ratio (canvas.width pixels maps to pdfWidth mm)
    const imgWidthMM = pdfWidth - 20; // leave 10mm margin each side
    const imgHeightMM = (canvas.height * imgWidthMM) / canvas.width;

    // center image horizontally with 10mm left margin
    pdf.addImage(imgData, 'PNG', 10, 10, imgWidthMM, imgHeightMM);
    pdf.save('My_CV.pdf');
  }
</script>
</body>
</html>
