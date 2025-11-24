# customer-details-form
[index.html](https://github.com/user-attachments/files/23722778/index.html)
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Customer Details Form</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
      background: radial-gradient(circle at 10% 10%, #e6f0ff, transparent 20%),
                  radial-gradient(circle at 90% 90%, #d9f7ff, transparent 20%),
                  linear-gradient(180deg, #a8c0ff, #3f2b96);
      color: #fff;
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .container {
      width: 100%;
      max-width: 520px;
      margin: 40px;
      background: rgba(255, 255, 255, 0.12);
      padding: 22px;
      border-radius: 22px;
      backdrop-filter: blur(8px) saturate(120%);
      box-shadow: 0 10px 30px rgba(31, 41, 55, 0.3);
      border: 1px solid rgba(255,255,255,0.06);
    }

    h2 {
      text-align: center;
      margin-bottom: 14px;
      font-size: 26px;
      letter-spacing: 1px;
    }

    label {
      font-size: 14px;
      font-weight: 600;
      display: block;
      margin-bottom: 6px;
      color: #eaf2ff;
    }

    input, textarea, select {
      width: 100%;
      padding: 12px;
      border-radius: 12px;
      border: none;
      outline: none;
      font-size: 14px;
      margin-bottom: 12px;
      background: rgba(255, 255, 255, 0.95);
      color: #0b1220;
      box-sizing: border-box;
    }

    .row {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
    }

    .full { grid-column: 1 / -1; }

    button {
      width: 100%;
      padding: 14px;
      font-size: 16px;
      border: none;
      border-radius: 14px;
      background: #ffffff;
      color: #3f2b96;
      font-weight: 700;
      cursor: pointer;
      transition: transform .15s ease;
    }

    button:active { transform: translateY(1px); }

    .success-msg {
      text-align: center;
      margin-top: 14px;
      font-size: 16px;
      display: none;
      color: #dff7e1;
    }

    .small { font-size: 12px; color: #dceaff; margin-top: -8px; margin-bottom: 10px; }
  </style>
</head>
<body>
  <div class="container">
    <h2>Customer Details</h2>
    <form id="detailsForm">
      <label>Your Full Name:</label>
      <input type="text" name="fullName" required />

      <div class="row">
        <div>
          <label>Date of Birth:</label>
          <input type="date" name="dob" required />
        </div>
        <div>
          <label>Age (auto):</label>
          <input type="text" id="age" readonly />
        </div>
      </div>

      <label>Your Wife's Name:</label>
      <input type="text" name="wifeName" />

      <div class="row">
        <div>
          <label>Wife Date of Birth:</label>
          <input type="date" name="wifeDob" />
        </div>
        <div>
          <label>Wife Age (auto):</label>
          <input type="text" id="wifeAge" readonly />
        </div>
      </div>

      <label>Your Children's Names (comma separated):</label>
      <input type="text" name="childrenNames" placeholder="e.g. Rahul, Priya" />

      <label>Children Ages (comma separated, match order):</label>
      <input type="text" name="childrenAge" placeholder="e.g. 5, 3" />

      <label>Address with Pin Code:</label>
      <textarea name="address" rows="2" required></textarea>

      <div class="row">
        <div>
          <label>Contact Number 1:</label>
          <input type="tel" name="contact1" required />
        </div>
        <div>
          <label>Contact Number 2:</label>
          <input type="tel" name="contact2" />
        </div>
      </div>

      <label>Occupation With Designation :</label>
      <input type="text" name="occupation" />

      <label>Email ID:</label>
      <input type="email" name="email" required />

      <label>Travel Dates:</label>
      <input type="text" name="travelDates" placeholder="e.g. 2025-12-03 to 2025-12-07" required />

      <button type="submit">Submit</button>
    </form>

    <div class="success-msg" id="successMsg">Your details have been submitted successfully!</div>
  </div>

  <script>
    // Helper: calculate age from date string (YYYY-MM-DD)
    function calcAge(dob) {
      if (!dob) return '';
      const b = new Date(dob);
      const today = new Date();
      let age = today.getFullYear() - b.getFullYear();
      const m = today.getMonth() - b.getMonth();
      if (m < 0 || (m === 0 && today.getDate() < b.getDate())) age--;
      return age;
    }

    document.querySelector('input[name="dob"]').addEventListener('change', function(e){
      document.getElementById('age').value = calcAge(e.target.value);
    });
    document.querySelector('input[name="wifeDob"]').addEventListener('change', function(e){
      document.getElementById('wifeAge').value = calcAge(e.target.value);
    });

    document.getElementById("detailsForm").addEventListener("submit", function (event) {
      event.preventDefault();

      const form = event.target;
      const data = {
        fullName: form.fullName.value,
        dob: form.dob.value,
        age: document.getElementById('age').value,
        wifeName: form.wifeName.value,
        wifeDob: form.wifeDob.value,
        wifeAge: document.getElementById('wifeAge').value,
        childrenNames: form.childrenNames.value,
        childrenAge: form.childrenAge.value,
        address: form.address.value,
        contact1: form.contact1.value,
        contact2: form.contact2.value,
        occupation: form.occupation.value,
        email: form.email.value,
        travelDates: form.travelDates.value,
        submittedAt: new Date().toISOString()
      };

      // Save to localStorage as table array
      const existing = JSON.parse(localStorage.getItem("customerData") || "[]");
      existing.push(data);
      localStorage.setItem("customerData", JSON.stringify(existing));

      // Show success and reset (keep small data if needed)
      document.getElementById("successMsg").style.display = "block";
      form.reset();
      document.getElementById('age').value = '';
      document.getElementById('wifeAge').value = '';
      setTimeout(()=> document.getElementById("successMsg").style.display = "none", 4000);
    });
  </script>
</body>
</html>
