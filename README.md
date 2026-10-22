<!DOCTYPE html>
<html lang="my">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My AI - App</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        body {
            background-color: #0d0f18;
            color: #ffffff;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }
        .mobile-frame {
            width: 100%;
            max-width: 420px;
            height: 100vh;
            max-height: 850px;
            background-color: #161925;
            border-radius: 24px;
            overflow: hidden;
            box-shadow: 0 20px 40px rgba(0,0,0,0.6);
            display: flex;
            flex-direction: column;
            border: 1px solid #2a2e3d;
        }
        .header {
            background-color: #1d2130;
            padding: 16px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #2d3245;
        }
        .logo {
            font-size: 22px;
            font-weight: bold;
            color: #00f2fe;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .badge {
            font-size: 11px;
            padding: 4px 10px;
            border-radius: 12px;
            font-weight: bold;
        }
        .badge-free {
            background-color: #3b2a1a;
            color: #ffb703;
            border: 1px solid #ffb70355;
        }
        .badge-pro {
            background-color: #1a3b2b;
            color: #00f5d4;
            border: 1px solid #00f5d455;
        }
        .pro-banner {
            background: linear-gradient(90deg, #ff007f, #7928ca);
            padding: 10px 16px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 12px;
        }
        .pro-btn {
            background: white;
            color: #7928ca;
            border: none;
            padding: 5px 12px;
            border-radius: 12px;
            font-weight: bold;
            cursor: pointer;
        }
        .tabs {
            display: flex;
            background-color: #1d2130;
            border-bottom: 1px solid #2d3245;
        }
        .tab-btn {
            flex: 1;
            padding: 12px 5px;
            background: none;
            border: none;
            color: #8f9bba;
            font-size: 12px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
        }
        .tab-btn.active {
            color: #00f2fe;
            border-bottom: 3px solid #00f2fe;
            background-color: rgba(0, 242, 254, 0.05);
        }
        .content {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
        }
        .section {
            display: none;
        }
        .section.active {
            display: block;
        }
        .card {
            background-color: #212638;
            border-radius: 16px;
            padding: 16px;
            margin-bottom: 16px;
            border: 1px solid #2d334a;
        }
        .card-title {
            font-size: 15px;
            margin-bottom: 12px;
            color: #00f2fe;
        }
        textarea, input[type="text"] {
            width: 100%;
            background-color: #141722;
            border: 1px solid #2d334a;
            border-radius: 10px;
            padding: 12px;
            color: white;
            font-size: 14px;
            resize: none;
            outline: none;
            margin-bottom: 12px;
        }
        .action-btn {
            width: 100%;
            padding: 12px;
            background: linear-gradient(90deg, #00f2fe, #4facfe);
            border: none;
            border-radius: 10px;
            color: #000;
            font-weight: bold;
            font-size: 14px;
            cursor: pointer;
            transition: opacity 0.2s;
        }
        .action-btn:hover {
            opacity: 0.9;
        }
        .result-box {
            margin-top: 15px;
            padding: 12px;
            background-color: #141722;
            border-radius: 10px;
            font-size: 13px;
            color: #ccc;
            min-height: 60px;
        }
        .image-preview {
            width: 100%;
            height: 180px;
            background-color: #141722;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            border: 2px dashed #2d334a;
            margin-bottom: 12px;
            cursor: pointer;
            overflow: hidden;
        }
        .image-preview img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        .modal {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.8);
            justify-content: center;
            align-items: center;
            z-index: 100;
        }
        .modal-content {
            background: #212638;
            padding: 24px;
            border-radius: 20px;
            max-width: 320px;
            text-align: center;
            border: 1px solid #00f2fe55;
        }
    </style>
</head>
<body>

<div class="mobile-frame">
    <!-- App Header -->
    <div class="header">
        <div class="logo">🤖 My AI</div>
        <div id="userBadge" class="badge badge-free">Free: <span id="limitCount">2</span>/2 ကျန်</div>
    </div>

    <!-- Pro Banner -->
    <div id="proBanner" class="pro-banner">
        <span>⚡ Pro တစ်လ ၃၀,၀၀၀ ကျပ် (အကန့်အသတ်မရှိ)</span>
        <button class="pro-btn" onclick="openProModal()">Upgrade</button>
    </div>

    <!-- App Tabs -->
    <div class="tabs">
        <button class="tab-btn active" onclick="switchTab('stt', this)">🎙️ အသံ ➔ စာ</button>
        <button class="tab-btn" onclick="switchTab('tts', this)">🔊 စာ ➔ အသံ</button>
        <button class="tab-btn" onclick="switchTab('img2vid', this)">🎬 Photo ➔ Video</button>
    </div>

    <!-- App Main Content -->
    <div class="content">
        <!-- 1. Speech to Text -->
        <div id="stt" class="section active">
            <div class="card">
                <div class="card-title">🎙️ Speech to Text (အသံမှ စာ)</div>
                <p style="font-size: 12px; color: #8f9bba; margin-bottom: 12px;">အသံဖြင့် ပြောဆိုပြီး စာသားသို့ ပြောင်းလဲပါ။ (9s Limit)</p>
                <button id="recordBtn" class="action-btn" onclick="toggleRecord()">🎤 အသံစတင်ပြောမည်</button>
                <div class="result-box" id="sttResult">စာသားများ ဒီမှာ ပေါ်လာမည်...</div>
            </div>
        </div>

        <!-- 2. Text to Speech / Gemini Voice -->
        <div id="tts" class="section">
            <div class="card">
                <div class="card-title">🔊 Text to Speech (Gemini Voice)</div>
                <textarea id="ttsInput" rows="4" placeholder="အသံပြောင်းလိုသော စာသားကို ရိုက်ထည့်ပါ..."></textarea>
                <button class="action-btn" onclick="generateVoice()">🔊 Gemini Voice ဖန်တီးမည် (9s)</button>
                <div class="result-box" id="ttsResult">အသံဖိုင် အခြေအနေ ဒီမှာ ပေါ်မည်...</div>
            </div>
        </div>

        <!-- 3. Image to Video -->
        <div id="img2vid" class="section">
            <div class="card">
                <div class="card-title">🎬 Image to Video (ဓာတ်ပုံမှ ဗီဒီယို)</div>
                <div class="image-preview" id="imgPreview" onclick="selectImage()">
                    <span id="imgText" style="color: #8f9bba; font-size: 13px;">📸 ဓာတ်ပုံ ရွေးချယ်ရန် နှိပ်ပါ</span>
                </div>
                <button class="action-btn" onclick="generateVideo()">🎥 9s Video ဖန်တီးမည်</button>
                <div class="result-box" id="videoResult">Video အထွက်ကို ဒီမှာ ပြသပါမည်...</div>
            </div>
        </div>
    </div>
</div>

<!-- Pro Upgrade Modal -->
<div id="proModal" class="modal">
    <div class="modal-content">
        <h3 style="color: #00f2fe; margin-bottom: 10px;">👑 My AI Pro VIP</h3>
        <p style="font-size: 13px; color: #ccc; margin-bottom: 15px;">တစ်လလျှင် <b>၃၀,၀၀၀ ကျပ်</b> ဖြင့် Daily Free Limit မလိုဘဲ အကန့်အသတ်မရှိ အသုံးပြုနိုင်ပါပြီ။</p>
        <p style="font-size: 12px; color: #8f9bba; margin-bottom: 20px;">KPay / WavePay: 09123456789</p>
        <button class="action-btn" onclick="confirmPro()" style="margin-bottom: 8px;">Upgrade ပြုလုပ်မည်</button>
        <button class="action-btn" onclick="closeProModal()" style="background: #333; color: white;">ပိတ်မည်</button>
    </div>
</div>

<script>
    let isPro = false;
    let freeCount = 2;
    let isRecording = false;
    let hasSelectedImg = false;

    function checkLimit() {
        if (isPro) return true;
        if (freeCount > 0) {
            freeCount--;
            document.getElementById('limitCount').innerText = freeCount;
            return true;
        } else {
            alert('ယနေ့အတွက် အခမဲ့ ၂ ကြိမ် သုံးပြီးပါပြီ! ဆက်သုံးရန် Pro Plan သို့ Upgrade လုပ်ပါ။');
            openProModal();
            return false;
        }
    }

    function switchTab(tabId, btn) {
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
        document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
        document.getElementById(tabId).classList.add('active');
        btn.classList.add('active');
    }

    function toggleRecord() {
        if (!isRecording) {
            if (!checkLimit()) return;
            isRecording = true;
            document.getElementById('recordBtn').innerText = '🔴 နားထောင်နေသည်... (ရပ်မည်)';
            document.getElementById('sttResult').innerText = 'အသံဖမ်းယူနေပါသည်။ ပြောဆိုပါ...';
            setTimeout(() => {
                document.getElementById('sttResult').innerText = 'မင်္ဂလာပါ။ My AI မှ ကြိုဆိုပါတယ်။ (Speech converted)';
                document.getElementById('recordBtn').innerText = '🎤 အသံစတင်ပြောမည်';
                isRecording = false;
            }, 3000);
        } else {
            isRecording = false;
            document.getElementById('recordBtn').innerText = '🎤 အသံစတင်ပြောမည်';
        }
    }

    function generateVoice() {
        const txt = document.getElementById('ttsInput').value;
        if (!txt) {
            alert('ကျေးဇူးပြု၍ စာသားရိုက်ထည့်ပါ');
            return;
        }
        if (!checkLimit()) return;
        document.getElementById('ttsResult').innerText = 'Gemini Voice ပြုလုပ်နေပါသည်...';
        setTimeout(() => {
            document.getElementById('ttsResult').innerText = '✅ 9s Gemini Voice ပြုလုပ်ပြီးပါပြီ! 🔊 [Play Audio]';
        }, 2000);
    }

    function selectImage() {
        hasSelectedImg = true;
        document.getElementById('imgPreview').innerHTML = '<img src="https://picsum.photos/400/250" alt="Selected">';
    }

    function generateVideo() {
        if (!hasSelectedImg) {
            alert('ဓာတ်ပုံတစ်ပုံ ရွေးချယ်ပါ');
            return;
        }
        if (!checkLimit()) return;
        document.getElementById('videoResult').innerText = '9s Video ဖန်တီးနေပါသည်...';
        setTimeout(() => {
            document.getElementById('videoResult').innerHTML = '✅ 9s AI Video ဖန်တီးပြီးပါပြီ!<br><br>🎬 [Generated AI Video Preview]';
        }, 3000);
    }

    function openProModal() { document.getElementById('proModal').style.display = 'flex'; }
    function closeProModal() { document.getElementById('proModal').style.display = 'none'; }
    function confirmPro() {
        isPro = true;
        document.getElementById('userBadge').className = 'badge badge-pro';
        document.getElementById('userBadge').innerText = 'PRO VIP';
        document.getElementById('proBanner').style.display = 'none';
        closeProModal();
        alert('ဝယ်ယူမှု အောင်မြင်ပါသည်။ My AI Pro VIP မှ ကြိုဆိုပါသည်!');
    }
</script>

</body>
</html>
