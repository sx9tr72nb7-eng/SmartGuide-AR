# SmartGuide-AR
Augmented Reality system for Smart University Guide – AR markers + locations + campus navigation
<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<title>AR إحداثيات الموقع</title>
<style>
    body {
        margin:0;
        overflow:hidden;
        font-family: Arial;
        background:#000;
        color:#fff;
    }

    #coords-box {
        position: fixed;
        bottom: 20px;
        left: 50%;
        transform: translateX(-50%);
        background: rgba(0,0,0,0.7);
        padding: 15px;
        border-radius: 10px;
        font-size: 20px;
        text-align: center;
        width: 90%;
    }

    video {
        width: 100%;
        height: 100vh;
        object-fit: cover;
    }
</style>
</head>

<body>

<video id="camera" autoplay playsinline></video>

<div id="coords-box">
    <div>📍 إحداثيات موقعك الآن:</div>
    <div id="lat">خط العرض: ...</div>
    <div id="lon">خط الطول: ...</div>
</div>

<script>
// تشغيل الكاميرا
async function startCamera() {
    try {
        const stream = await navigator.mediaDevices.getUserMedia({
            video: { facingMode: 'environment' }
        });
        document.getElementById('camera').srcObject = stream;
    } catch (err) {
        alert("لا يمكن تشغيل الكاميرا: " + err);
    }
}

// جلب الإحداثيات كل 1 ثانية
function getLocation() {
    if (!navigator.geolocation) {
        alert("المتصفح لا يدعم GPS");
        return;
    }

    navigator.geolocation.watchPosition(pos => {
        document.getElementById('lat').innerText = "خط العرض: " + pos.coords.latitude;
        document.getElementById('lon').innerText = "خط الطول: " + pos.coords.longitude;
    },
    err => {
        alert("فشل تحديد الموقع: " + err.message);
    },
    {
        enableHighAccuracy: true
    });
}

startCamera();
getLocation();
</script>

</body>
</html>
