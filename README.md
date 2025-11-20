<!DOCTYPE html>
<html lang="ms">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Borang Resume Digital / Digital Resume Form</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- HTML2PDF Library -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap');

        body {
            font-family: 'Inter', sans-serif;
            /* Background inspired by Malaysia (Red, Blue, Yellow) and Sabah (Blue, White, Red) flags */
            background: linear-gradient(135deg, #0032A0 0%, #ffffff 50%, #CC0000 100%);
            background-attachment: fixed;
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f1f1; 
        }
        ::-webkit-scrollbar-thumb {
            background: #0032A0; 
            border-radius: 4px;
        }

        /* Print specific adjustments to ensure PDF looks good */
        .print-hidden {
            display: block;
        }
        
        .form-input {
            transition: all 0.3s ease;
        }
        .form-input:focus {
            border-color: #0032A0;
            box-shadow: 0 0 0 3px rgba(0, 50, 160, 0.2);
        }

        /* Decor strips */
        .flag-strip {
            height: 8px;
            width: 100%;
            background: linear-gradient(90deg, #0032A0 25%, #EF3340 25%, #EF3340 50%, #FFD100 50%, #FFD100 75%, #ffffff 75%);
        }
    </style>
</head>
<body class="min-h-screen py-8 px-4">

    <!-- Main Container -->
    <div class="max-w-4xl mx-auto bg-white shadow-2xl rounded-xl overflow-hidden border border-gray-200 relative">
        
        <!-- Top Flag Decoration -->
        <div class="flag-strip"></div>

        <!-- Controls (Language & Actions) - Hidden in PDF -->
        <div class="bg-gray-50 p-4 border-b flex justify-between items-center print-hidden" id="controls-area">
            <div class="flex items-center gap-2">
                <i class="fa-solid fa-globe text-blue-800"></i>
                <span class="font-bold text-gray-700 text-sm sm:text-base">Language / Bahasa:</span>
                <button onclick="setLanguage('ms')" class="px-3 py-1 bg-blue-600 text-white text-xs sm:text-sm rounded hover:bg-blue-700 transition">BM</button>
                <button onclick="setLanguage('en')" class="px-3 py-1 bg-red-600 text-white text-xs sm:text-sm rounded hover:bg-red-700 transition">ENG</button>
            </div>
            <div class="text-xs text-gray-500 hidden sm:block">
                Sistem Penjanaan Resume Digital
            </div>
        </div>

        <!-- Resume Content Area (This part gets printed) -->
        <div id="resume-content" class="p-8 md:p-12 bg-white">
            
            <!-- Header Section -->
            <div class="flex flex-col md:flex-row gap-8 items-center border-b-2 border-gray-200 pb-8 mb-8">
                <!-- Photo Upload Slot -->
                <div class="relative group">
                    <div class="w-32 h-32 md:w-40 md:h-40 rounded-full overflow-hidden border-4 border-blue-900 bg-gray-100 flex justify-center items-center shadow-lg">
                        <img id="profile-preview" src="https://via.placeholder.com/150?text=Foto" class="w-full h-full object-cover" alt="Profile Photo">
                        <div class="absolute inset-0 bg-black bg-opacity-40 flex items-center justify-center opacity-0 group-hover:opacity-100 transition cursor-pointer" onclick="document.getElementById('photo-upload').click()">
                            <i class="fa-solid fa-camera text-white text-2xl"></i>
                        </div>
                    </div>
                    <input type="file" id="photo-upload" accept="image/*" class="hidden" onchange="previewImage(event)">
                    <p class="text-center text-xs text-gray-500 mt-2 data-en hidden">Click to upload</p>
                    <p class="text-center text-xs text-gray-500 mt-2 data-ms">Klik untuk muat naik</p>
                </div>

                <!-- Header Inputs -->
                <div class="flex-1 w-full text-center md:text-left">
                    <h1 class="text-3xl font-bold text-gray-800 mb-2">
                        <input type="text" placeholder="NAMA PENUH / FULL NAME" class="w-full border-b-2 border-gray-300 focus:border-blue-600 outline-none py-1 bg-transparent font-bold text-gray-800 placeholder-gray-400 uppercase text-2xl md:text-3xl text-center md:text-left">
                    </h1>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mt-4">
                        <div class="flex items-center gap-2 text-gray-600">
                            <i class="fa-solid fa-briefcase w-5"></i>
                            <input type="text" placeholder="Jawatan Dipohon / Position Applied" class="w-full border-b border-gray-200 focus:border-blue-500 outline-none py-1 bg-transparent text-sm">
                        </div>
                        <div class="flex items-center gap-2 text-gray-600">
                            <i class="fa-solid fa-phone w-5"></i>
                            <input type="tel" placeholder="No. Telefon / Phone No." class="w-full border-b border-gray-200 focus:border-blue-500 outline-none py-1 bg-transparent text-sm">
                        </div>
                        <div class="flex items-center gap-2 text-gray-600">
                            <i class="fa-solid fa-envelope w-5"></i>
                            <input type="email" placeholder="Email Address" class="w-full border-b border-gray-200 focus:border-blue-500 outline-none py-1 bg-transparent text-sm">
                        </div>
                        <div class="flex items-center gap-2 text-gray-600">
                            <i class="fa-solid fa-location-dot w-5"></i>
                            <input type="text" placeholder="Alamat / Address" class="w-full border-b border-gray-200 focus:border-blue-500 outline-none py-1 bg-transparent text-sm">
                        </div>
                    </div>
                </div>
            </div>

            <!-- Section 1: Personal Details -->
            <div class="mb-8">
                <h2 class="text-xl font-bold text-blue-900 border-l-4 border-yellow-400 pl-3 mb-4 uppercase">
                    <span class="data-ms">Maklumat Peribadi</span>
                    <span class="data-en hidden">Personal Details</span>
                </h2>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <div>
                        <label class="block text-xs font-bold text-gray-500 uppercase mb-1">IC / Passport</label>
                        <input type="text" class="w-full p-2 border rounded bg-gray-50 focus:bg-white form-input">
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-gray-500 uppercase mb-1">
                            <span class="data-ms">Tarikh Lahir</span>
                            <span class="data-en hidden">Date of Birth</span>
                        </label>
                        <input type="date" class="w-full p-2 border rounded bg-gray-50 focus:bg-white form-input">
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-gray-500 uppercase mb-1">
                            <span class="data-ms">Warganegara</span>
                            <span class="data-en hidden">Nationality</span>
                        </label>
                        <input type="text" value="Malaysia" class="w-full p-2 border rounded bg-gray-50 focus:bg-white form-input">
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-gray-500 uppercase mb-1">
                            <span class="data-ms">Jantina</span>
                            <span class="data-en hidden">Gender</span>
                        </label>
                        <select class="w-full p-2 border rounded bg-gray-50 focus:bg-white form-input">
                            <option value="">- Select -</option>
                            <option value="Lelaki">Lelaki / Male</option>
                            <option value="Perempuan">Perempuan / Female</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-gray-500 uppercase mb-1">
                            <span class="data-ms">Status</span>
                            <span class="data-en hidden">Marital Status</span>
                        </label>
                        <select class="w-full p-2 border rounded bg-gray-50 focus:bg-white form-input">
                            <option value="Bujang">Bujang / Single</option>
                            <option value="Berkahwin">Berkahwin / Married</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-bold text-gray-500 uppercase mb-1">
                            <span class="data-ms">Bangsa/Kaum</span>
                            <span class="data-en hidden">Race/Ethnicity</span>
                        </label>
                        <input type="text" class="w-full p-2 border rounded bg-gray-50 focus:bg-white form-input">
                    </div>
                </div>
            </div>

            <!-- Section 2: Education -->
            <div class="mb-8">
                <h2 class="text-xl font-bold text-blue-900 border-l-4 border-red-600 pl-3 mb-4 uppercase">
                    <span class="data-ms">Pendidikan</span>
                    <span class="data-en hidden">Education</span>
                </h2>
                <div class="space-y-4">
                    <!-- Row 1 -->
                    <div class="grid grid-cols-1 md:grid-cols-12 gap-4 items-start">
                        <div class="md:col-span-3">
                            <input type="text" placeholder="Tahun / Year" class="w-full p-2 border rounded bg-gray-50 text-center font-bold text-gray-700">
                        </div>
                        <div class="md:col-span-9">
                            <input type="text" placeholder="Institusi / Institution (e.g. UMS, UiTM, SMK)" class="w-full p-2 border-b border-gray-300 focus:border-blue-500 outline-none font-semibold mb-1">
                            <input type="text" placeholder="Kelayakan / Qualification (e.g. SPM, Diploma, Degree)" class="w-full p-1 text-sm text-gray-600 border-b border-gray-200 outline-none">
                        </div>
                    </div>
                    <!-- Row 2 -->
                    <div class="grid grid-cols-1 md:grid-cols-12 gap-4 items-start">
                        <div class="md:col-span-3">
                            <input type="text" placeholder="Tahun / Year" class="w-full p-2 border rounded bg-gray-50 text-center font-bold text-gray-700">
                        </div>
                        <div class="md:col-span-9">
                            <input type="text" placeholder="Institusi / Institution" class="w-full p-2 border-b border-gray-300 focus:border-blue-500 outline-none font-semibold mb-1">
                            <input type="text" placeholder="Kelayakan / Qualification" class="w-full p-1 text-sm text-gray-600 border-b border-gray-200 outline-none">
                        </div>
                    </div>
                </div>
            </div>

            <!-- Section 3: Working Experience -->
            <div class="mb-8">
                <h2 class="text-xl font-bold text-blue-900 border-l-4 border-blue-400 pl-3 mb-4 uppercase">
                    <span class="data-ms">Pengalaman Kerja</span>
                    <span class="data-en hidden">Working Experience</span>
                </h2>
                <div class="space-y-6">
                    <!-- Job 1 -->
                    <div class="bg-gray-50 p-4 rounded-lg border border-gray-100">
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-2">
                            <input type="text" placeholder="Nama Syarikat / Company Name" class="font-bold text-gray-800 bg-transparent border-b border-gray-300 w-full outline-none">
                            <input type="text" placeholder="Tempoh / Duration (e.g. 2020 - 2023)" class="text-right text-gray-600 bg-transparent border-b border-gray-300 w-full outline-none text-sm">
                        </div>
                        <input type="text" placeholder="Jawatan / Position Title" class="w-full text-blue-800 font-semibold bg-transparent outline-none mb-2">
                        <textarea rows="2" placeholder="Skop Kerja / Job Description..." class="w-full p-2 text-sm text-gray-600 bg-white border rounded resize-none"></textarea>
                    </div>
                     <!-- Job 2 -->
                     <div class="bg-gray-50 p-4 rounded-lg border border-gray-100">
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-2">
                            <input type="text" placeholder="Nama Syarikat / Company Name" class="font-bold text-gray-800 bg-transparent border-b border-gray-300 w-full outline-none">
                            <input type="text" placeholder="Tempoh / Duration" class="text-right text-gray-600 bg-transparent border-b border-gray-300 w-full outline-none text-sm">
                        </div>
                        <input type="text" placeholder="Jawatan / Position Title" class="w-full text-blue-800 font-semibold bg-transparent outline-none mb-2">
                        <textarea rows="2" placeholder="Skop Kerja / Job Description..." class="w-full p-2 text-sm text-gray-600 bg-white border rounded resize-none"></textarea>
                    </div>
                </div>
            </div>

            <!-- Section 4: Skills & Language -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8 mb-8">
                <!-- Skills -->
                <div>
                    <h2 class="text-xl font-bold text-blue-900 border-l-4 border-yellow-400 pl-3 mb-4 uppercase">
                        <span class="data-ms">Kemahiran</span>
                        <span class="data-en hidden">Skills</span>
                    </h2>
                    <textarea rows="5" class="w-full p-3 border rounded-lg bg-gray-50 text-sm" placeholder="- Microsoft Office (Word, Excel)&#10;- Pengurusan Masa&#10;- Kerja Berpasukan&#10;- Memandu (Lesen D)"></textarea>
                </div>
                <!-- Languages -->
                <div>
                    <h2 class="text-xl font-bold text-blue-900 border-l-4 border-red-600 pl-3 mb-4 uppercase">
                        <span class="data-ms">Bahasa</span>
                        <span class="data-en hidden">Languages</span>
                    </h2>
                    <div class="space-y-2">
                        <div class="flex items-center justify-between border-b py-2">
                            <span>Bahasa Malaysia</span>
                            <select class="bg-gray-100 text-xs rounded p-1">
                                <option>Cemerlang / Excellent</option>
                                <option>Baik / Good</option>
                                <option>Sederhana / Fair</option>
                            </select>
                        </div>
                        <div class="flex items-center justify-between border-b py-2">
                            <span>English</span>
                            <select class="bg-gray-100 text-xs rounded p-1">
                                <option>Cemerlang / Excellent</option>
                                <option>Baik / Good</option>
                                <option>Sederhana / Fair</option>
                            </select>
                        </div>
                        <div class="flex items-center justify-between border-b py-2">
                            <input type="text" placeholder="Lain-lain / Others" class="bg-transparent outline-none text-sm w-1/2">
                            <select class="bg-gray-100 text-xs rounded p-1">
                                <option>Baik / Good</option>
                                <option>Sederhana / Fair</option>
                            </select>
                        </div>
                    </div>
                </div>
            </div>

             <!-- Section 5: References -->
             <div class="mb-4">
                <h2 class="text-xl font-bold text-blue-900 border-l-4 border-blue-400 pl-3 mb-4 uppercase">
                    <span class="data-ms">Rujukan</span>
                    <span class="data-en hidden">References</span>
                </h2>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div class="p-3 border border-dashed border-gray-300 rounded">
                        <input type="text" placeholder="Nama Rujukan 1 / Reference Name 1" class="w-full font-bold text-sm mb-1 outline-none">
                        <input type="text" placeholder="Jawatan & Syarikat / Position & Company" class="w-full text-xs text-gray-600 mb-1 outline-none">
                        <input type="text" placeholder="No. Telefon / Phone No." class="w-full text-xs text-blue-600 outline-none">
                    </div>
                    <div class="p-3 border border-dashed border-gray-300 rounded">
                        <input type="text" placeholder="Nama Rujukan 2 / Reference Name 2" class="w-full font-bold text-sm mb-1 outline-none">
                        <input type="text" placeholder="Jawatan & Syarikat / Position & Company" class="w-full text-xs text-gray-600 mb-1 outline-none">
                        <input type="text" placeholder="No. Telefon / Phone No." class="w-full text-xs text-blue-600 outline-none">
                    </div>
                </div>
            </div>

            <!-- Footer -->
            <div class="text-center mt-8 pt-4 border-t border-gray-200">
                <p class="text-xs text-gray-400">Generated by Digital Resume System</p>
            </div>
        </div>

        <!-- Bottom Flag Decoration -->
        <div class="flag-strip"></div>
    </div>

    <!-- Action Buttons (Fixed Bottom) -->
    <div class="fixed bottom-0 left-0 w-full bg-white shadow-lg border-t p-4 flex justify-center gap-4 z-50 print-hidden">
        <button onclick="resetForm()" class="bg-gray-500 hover:bg-gray-600 text-white font-bold py-2 px-6 rounded-full shadow-lg transition transform hover:scale-105 flex items-center gap-2">
            <i class="fa-solid fa-rotate-left"></i> 
            <span class="data-ms">Reset</span>
            <span class="data-en hidden">Reset</span>
        </button>
        <button onclick="generateAndEmail()" class="bg-blue-700 hover:bg-blue-800 text-white font-bold py-2 px-6 rounded-full shadow-lg transition transform hover:scale-105 flex items-center gap-2">
            <i class="fa-solid fa-paper-plane"></i>
            <span class="data-ms">Simpan & Email</span>
            <span class="data-en hidden">Save & Email</span>
        </button>
    </div>

    <!-- JavaScript Functionality -->
    <script>
        // Language Switcher
        function setLanguage(lang) {
            const msElements = document.querySelectorAll('.data-ms');
            const enElements = document.querySelectorAll('.data-en');

            if (lang === 'ms') {
                msElements.forEach(el => el.classList.remove('hidden'));
                enElements.forEach(el => el.classList.add('hidden'));
                document.documentElement.lang = 'ms';
            } else {
                msElements.forEach(el => el.classList.add('hidden'));
                enElements.forEach(el => el.classList.remove('hidden'));
                document.documentElement.lang = 'en';
            }
        }

        // Image Preview
        function previewImage(event) {
            const reader = new FileReader();
            reader.onload = function() {
                const output = document.getElementById('profile-preview');
                output.src = reader.result;
            }
            reader.readAsDataURL(event.target.files[0]);
        }

        // Reset Form
        function resetForm() {
            if(confirm("Adakah anda pasti mahu memadam semua maklumat? / Are you sure you want to clear the form?")) {
                document.querySelectorAll('input').forEach(input => input.value = '');
                document.querySelectorAll('textarea').forEach(area => area.value = '');
                document.querySelectorAll('select').forEach(sel => sel.selectedIndex = 0);
                document.getElementById('profile-preview').src = "https://via.placeholder.com/150?text=Foto";
            }
        }

        // Generate PDF and Trigger Email
        function generateAndEmail() {
            const element = document.getElementById('resume-content');
            const nameInput = document.querySelector('input[placeholder="NAMA PENUH / FULL NAME"]').value || "Pemohon";
            const filename = `Resume_${nameInput.replace(/\s+/g, '_')}.pdf`;
            const emailTo = "moyedin.abdullah@gmail.com";
            
            // PDF Configuration
            const opt = {
                margin:       0,
                filename:     filename,
                image:        { type: 'jpeg', quality: 0.98 },
                html2canvas:  { scale: 2, useCORS: true },
                jsPDF:        { unit: 'in', format: 'a4', orientation: 'portrait' }
            };

            // Show loading state
            const btn = document.querySelector('button[onclick="generateAndEmail()"]');
            const originalText = btn.innerHTML;
            btn.innerHTML = '<i class="fa-solid fa-spinner fa-spin"></i> Processing...';
            btn.disabled = true;

            // 1. Generate and Download PDF
            html2pdf().set(opt).from(element).save().then(() => {
                
                // 2. Reset Button
                btn.innerHTML = originalText;
                btn.disabled = false;

                // 3. Construct Mailto Link
                const subject = encodeURIComponent(`Permohonan Kerja: ${nameInput}`);
                const body = encodeURIComponent(`Salam Sejahtera,\n\nSila dapati resume saya dilampirkan dalam email ini bagi tujuan permohonan kerja.\n\nSekian, terima kasih.\n\n${nameInput}`);
                
                const mailtoLink = `mailto:${emailTo}?subject=${subject}&body=${body}`;

                // 4. Open Email Client
                window.location.href = mailtoLink;

                // 5. Alert User (Crucial Step because browsers can't auto-attach)
                setTimeout(() => {
                    alert(`PDF (${filename}) telah dimuat turun ke komputer/telefon anda.\n\nSistem email anda telah dibuka. Sila 'Attach' fail PDF yang baru dimuat turun itu secara manual sebelum menghantar email.`);
                }, 1000);

            }).catch(err => {
                console.error(err);
                alert("Ralat semasa menjana PDF. Sila cuba lagi.");
                btn.innerHTML = originalText;
                btn.disabled = false;
            });
        }
    </script>
</body>
</html>
