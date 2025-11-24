<script>
  const WEBAPP_URL = "YOUR_WEBAPP_URL_HERE";  // paste your URL inside quotes

  document.getElementById("detailsForm").addEventListener("submit", async function(event) {
    event.preventDefault();

    const form = event.target;

    const payload = {
      fullName: form.fullName.value,
      dob: form.dob.value,
      age: document.getElementById("age").value,
      wifeName: form.wifeName.value,
      wifeDob: form.wifeDob.value,
      wifeAge: document.getElementById("wifeAge").value,
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

    try {
      const response = await fetch(WEBAPP_URL, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(payload)
      });

      const result = await response.json();

      if (result.status === "success") {
        document.getElementById("successMsg").style.display = "block";
        form.reset();
        document.getElementById("age").value = "";
        document.getElementById("wifeAge").value = "";

        setTimeout(() => {
          document.getElementById("successMsg").style.display = "none";
        }, 3000);
      } else {
        alert("Error saving data.");
        console.log(result);
      }

    } catch (error) {
      alert("Failed to submit form. Check console.");
      console.error(error);
    }

  });
</script>
