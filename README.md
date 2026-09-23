# -ADMISSION-PORTAL-
<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Student Admission & Automated Receipt Generator</title>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@500;600;700;800;900&display=swap" rel="stylesheet">
  
  <!-- html2pdf bundle for automatic PDF download -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>

  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Plus Jakarta Sans', sans-serif;
    }

    body {
      background: #f1f5f9;
      color: #0f172a;
      padding: 20px 10px;
    }

    .container {
      max-width: 820px;
      margin: 0 auto;
    }

    /* CARD WRAPPER */
    .form-box {
      background: #ffffff;
      border: 2px solid #cbd5e1;
      border-radius: 14px;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
      padding: 28px;
      margin-bottom: 25px;
    }

    .head-title {
      font-size: 24px;
      font-weight: 900;
      color: #0369a1;
      border-bottom: 2px solid #0284c7;
      padding-bottom: 10px;
      margin-bottom: 20px;
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .form-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 16px;
    }

    .form-group {
      display: flex;
      flex-direction: column;
      gap: 6px;
    }

    .form-group.full {
      grid-column: 1 / -1;
    }

    label {
      font-size: 13px;
      font-weight: 700;
      color: #334155;
    }

    input, select, textarea {
      width: 100%;
      padding: 10px 12px;
      border: 1.5px solid #cbd5e1;
      border-radius: 6px;
      font-size: 14px;
      font-weight: 600;
      color: #0f172a;
      outline: none;
      transition: border 0.2s;
    }

    input:focus, select:focus, textarea:focus {
      border-color: #0284c7;
      box-shadow: 0 0 0 3px rgba(2, 132, 199, 0.15);
    }

    /* BUTTONS */
    .btn-row {
      display: flex;
      gap: 12px;
      margin-top: 20px;
    }

    .submit-btn {
      flex: 1;
      background: #0284c7;
      color: #ffffff;
      font-size: 15px;
      font-weight: 800;
      padding: 12px;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 8px;
      transition: background 0.2s;
    }

    .submit-btn:hover {
      background: #0369a1;
    }

    /* PREVIEW & PDF TEMPLATE */
    #pdf-receipt-area {
      display: none;
      background: #ffffff;
      border: 2px solid #0284c7;
      border-radius: 12px;
      padding: 30px;
      margin-top: 25px;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
    }

    .receipt-header {
      text-align: center;
      border-bottom: 2px solid #0284c7;
      padding-bottom: 15px;
      margin-bottom: 20px;
    }

    .receipt-header h2 {
      font-size: 24px;
      font-weight: 900;
      color: #0f172a;
    }

    .receipt-header p {
      font-size: 13px;
      color: #64748b;
      font-weight: 600;
    }

    .receipt-badge {
      display: inline-block;
      background: #ecfdf5;
      color: #047857;
      border: 1px solid #10b981;
      font-size: 12px;
      font-weight: 800;
      padding: 4px 14px;
      border-radius: 20px;
      margin-top: 8px;
    }

    .receipt-table {
      width: 100%;
      border-collapse: collapse;
      margin-bottom: 20px;
    }

    .receipt-table th, .receipt-table td {
      border: 1px solid #cbd5e1;
      padding: 10px 14px;
      font-size: 13px;
      text-align: left;
    }

    .receipt-table th {
      background: #f8fafc;
      font-weight: 700;
      color: #475569;
      width: 32%;
    }

    .receipt-table td {
      font-weight: 800;
      color: #0f172a;
    }

    .receipt-footer-sign {
      display: flex;
      justify-content: space-between;
      margin-top: 40px;
      padding-top: 20px;
      border-top: 1px dashed #cbd5e1;
      font-size: 12px;
      color: #475569;
    }

    /* ACTION BUTTONS POST SUBMISSION */
    .post-action-bar {
      display: none;
      gap: 12px;
      margin-top: 16px;
    }

    .action-btn {
      flex: 1;
      padding: 12px;
      border-radius: 6px;
      font-size: 14px;
      font-weight: 800;
      cursor: pointer;
      text-decoration: none;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      border: none;
    }

    .btn-pdf {
      background: #dc2626;
      color: #ffffff;
    }

    .btn-pdf:hover {
      background: #b91c1c;
    }

    .btn-wa {
      background: #16a34a;
      color: #ffffff;
    }

    .btn-wa:hover {
      background: #15803d;
    }

    .btn-sms {
      background: #2563eb;
      color: #ffffff;
    }

    .btn-sms:hover {
      background: #1d4ed8;
    }

    @media (max-width: 600px) {
      .form-grid {
        grid-template-columns: 1fr;
      }
      .post-action-bar {
        flex-direction: column;
      }
    }
  </style>
</head>
<body>

  <div class="container">
    
    <!-- 1. ADMISSION ENTRY FORM -->
    <div class="form-box">
      <div class="head-title">
        <span>📝</span> New Student Admission Form
      </div>

      <form id="admissionForm">
        <div class="form-grid">
          
          <div class="form-group">
            <label>Student Full Name (छात्र का नाम) *</label>
            <input type="text" id="stuName" placeholder="उदा. Rahul Kumar" required>
          </div>

          <div class="form-group">
            <label>Father's Name (पिता का नाम) *</label>
            <input type="text" id="fatherName" placeholder="उदा. Ramesh Prasad" required>
          </div>

          <div class="form-group">
            <label>Class / Course (कक्षा / कोर्स) *</label>
            <select id="stuClass" required>
              <option value="">-- चुनें (Select Class) --</option>
              <option value="Class 8th">Class 8th</option>
              <option value="Class 9th">Class 9th</option>
              <option value="Class 10th (Matric)">Class 10th (Matric)</option>
              <option value="Class 11th (Science/Arts/Com)">Class 11th</option>
              <option value="Class 12th (Inter)">Class 12th (Inter)</option>
              <option value="Special Batch / English Speaking">Special Coaching Batch</option>
            </select>
          </div>

          <div class="form-group">
            <label>Roll No. / Student ID *</label>
            <input type="text" id="stuRoll" placeholder="उदा. 101" required>
          </div>

          <div class="form-group">
            <label>Parents Mobile / WhatsApp (फोन नंबर) *</label>
            <input type="tel" id="mobileNum" placeholder="उदा. 9876543210" maxlength="10" required>
          </div>

          <div class="form-group">
            <label>Admission Fee Paid (जमा शुल्क) *</label>
            <input type="number" id="feePaid" placeholder="₹ उदा. 500" required>
          </div>

          <div class="form-group full">
            <label>Address (स्थायी पता) *</label>
            <textarea id="stuAddress" rows="2" placeholder="उदा. Village, Post, Musrigharari, Samastipur" required></textarea>
          </div>

        </div>

        <div class="btn-row">
          <button type="submit" class="submit-btn">
            <span>✅</span> सबमिट करें &amp; रसीद तैयार करें
          </button>
        </div>
      </form>
    </div>

    <!-- 2. POST SUBMISSION ACTION BAR (WhatsApp, SMS, PDF) -->
    <div class="post-action-bar" id="actionBar">
      <button class="action-btn btn-pdf" id="downloadPdfBtn">
        <span>📥</span> Download Admission PDF
      </button>

      <a href="#" class="action-btn btn-wa" id="sendWaBtn" target="_blank">
        <span>💬</span> Send WhatsApp Confirmation
      </a>

      <a href="#" class="action-btn btn-sms" id="sendSmsBtn">
        <span>📩</span> Send Mobile SMS
      </a>
    </div>

    <!-- 3. PRINTABLE / AUTO-GENERATED PDF RECEIPT -->
    <div id="pdf-receipt-area">
      <div class="receipt-header">
        <h2>MS DIGITAL HUB &amp; COACHING CENTRE</h2>
        <p>Musrigharari, Samastipur, Bihar | Helpline: +91 9693345819</p>
        <div class="receipt-badge">✔ OFFICIAL ADMISSION CONFIRMATION SLIP</div>
      </div>

      <table class="receipt-table">
        <tr>
          <th>Admission Slip No:</th>
          <td id="rcptNo">MSD-2026-001</td>
        </tr>
        <tr>
          <th>Admission Date:</th>
          <td id="rcptDate">--/--/----</td>
        </tr>
        <tr>
          <th>Student Name (नाम):</th>
          <td id="rcptName">--</td>
        </tr>
        <tr>
          <th>Father's Name (पिता का नाम):</th>
          <td id="rcptFather">--</td>
        </tr>
        <tr>
          <th>Class / Course (कक्षा):</th>
          <td id="rcptClass">--</td>
        </tr>
        <tr>
          <th>Roll No / Student ID:</th>
          <td id="rcptRoll">--</td>
        </tr>
        <tr>
          <th>Contact Number:</th>
          <td id="rcptMobile">--</td>
        </tr>
        <tr>
          <th>Admission Fee Received:</th>
          <td style="color: #047857;" id="rcptFee">₹ 0</td>
        </tr>
        <tr>
          <th>Address:</th>
          <td id="rcptAddress">--</td>
        </tr>
      </table>

      <div style="background: #f8fafc; border: 1px solid #cbd5e1; padding: 10px; border-radius: 6px; font-size: 11.5px; color: #475569; line-height: 1.5;">
        <strong>नियम एवं शर्तें:</strong> छात्र का नामांकन संस्थान के नियमों के अनुसार सफलतापूर्वक पूरा हो चुका है। परीक्षा रिज़ल्ट, नोटिस और मार्कशीट ऑनलाइन पोर्टल (<strong>urdufor10th-png.github.io/Result-Portal-/</strong>) पर उपलब्ध रहेगी।
      </div>

      <div class="receipt-footer-sign">
        <div>Authorized By: <strong>MD SHAHABUDDIN</strong></div>
        <div>Director / Office Seal &amp; Signature</div>
      </div>
    </div>

  </div>

  <script>
    const form = document.getElementById('admissionForm');
    const pdfArea = document.getElementById('pdf-receipt-area');
    const actionBar = document.getElementById('actionBar');

    let currentStudent = {};

    form.addEventListener('submit', function(e) {
      e.preventDefault();

      // Collect data
      const name = document.getElementById('stuName').value.trim();
      const father = document.getElementById('fatherName').value.trim();
      const sClass = document.getElementById('stuClass').value;
      const roll = document.getElementById('stuRoll').value.trim();
      const mobile = document.getElementById('mobileNum').value.trim();
      const fee = document.getElementById('feePaid').value.trim();
      const address = document.getElementById('stuAddress').value.trim();

      const today = new Date();
      const dateStr = today.toLocaleDateString('en-GB', { day: '2-digit', month: 'short', year: 'numeric' });
      const receiptNo = 'MSD-' + Math.floor(1000 + Math.random() * 9000);

      currentStudent = { name, father, sClass, roll, mobile, fee, address, dateStr, receiptNo };

      // Fill in receipt area
      document.getElementById('rcptNo').textContent = receiptNo;
      document.getElementById('rcptDate').textContent = dateStr;
      document.getElementById('rcptName').textContent = name;
      document.getElementById('rcptFather').textContent = father;
      document.getElementById('rcptClass').textContent = sClass;
      document.getElementById('rcptRoll').textContent = roll;
      document.getElementById('rcptMobile').textContent = '+91 ' + mobile;
      document.getElementById('rcptFee').textContent = '₹ ' + fee + ' (Paid)';
      document.getElementById('rcptAddress').textContent = address;

      // Show receipt and action buttons
      pdfArea.style.display = 'block';
      actionBar.style.display = 'flex';

      // Setup WhatsApp Confirmation Link with Pre-filled Message
      const waMsg = encodeURIComponent(
        `🎓 *MS DIGITAL HUB & COACHING CENTRE*\n` +
        `प्रिय अभिभावक, आपके बच्चे *${name}* (Roll: ${roll}) का *${sClass}* में नामांकन सफलतापूर्वक हो गया है।\n` +
        `रसीद संख्या: ${receiptNo}\n` +
        `जमा शुल्क: ₹${fee}\n` +
        `ऑनलाइन पोर्टल: https://urdufor10th-png.github.io/Result-Portal-/\n` +
        `धन्यवाद! (निदेशक: MD Shahabuddin)`
      );
      document.getElementById('sendWaBtn').href = `https://wa.me/91${mobile}?text=${waMsg}`;

      // Setup Mobile SMS Link (native sms: URI)
      const smsBody = encodeURIComponent(
        `Badhai ho! Aapke bacche ${name} ka admission ${sClass} (Roll: ${roll}) me ho chuka hai. Receipt No: ${receiptNo}. Helpline: 9693345819 - MS Digital Hub`
      );
      document.getElementById('sendSmsBtn').href = `sms:${mobile}?body=${smsBody}`;

      // Automatically trigger PDF download
      downloadPDF();
    });

    // Auto PDF Generator Function
    function downloadPDF() {
      const element = document.getElementById('pdf-receipt-area');
      const opt = {
        margin:       10,
        filename:     `Admission_Receipt_${currentStudent.name || 'Student'}.pdf`,
        image:        { type: 'jpeg', quality: 0.98 },
        html2canvas:  { scale: 2 },
        jsPDF:        { unit: 'mm', format: 'a4', orientation: 'portrait' }
      };

      html2pdf().set(opt).from(element).save();
    }

    document.getElementById('downloadPdfBtn').addEventListener('click', downloadPDF);
  </script>

</body>
</html>
