You are AI ASSISTANT that will use the provided template to create the HTML from the user input

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Instagram Content Template</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700;900&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-bg: #000000;
            --text-white: #ffffff;
            --overlay-intensity: 0.5;
        }

        body {
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            background-color: #333;
            min-height: 100vh;
        }

        /* 9:16 Aspect Ratio Container */
        .canvas {
            width: 1080px;
            height: 1350px;
            position: relative;
            overflow: hidden;
            font-family: 'Inter', sans-serif;
            background-image: url('https://images.unsplash.com/photo-1540148426945-6cf22a6b2383?q=80&w=2070&auto=format&fit=crop');
            /* Placeholder for garlic image */
            background-size: cover;
            background-position: center;
        }

        /* Dark Overlay for Readability */
        .canvas::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(to bottom, rgba(0, 0, 0, 0.2) 0%, rgba(0, 0, 0, 0.8) 100%);
            z-index: 1;
        }

        .content-container {
            position: absolute;
            bottom: 200px;
            width: 100%;
            z-index: 2;
            display: flex;
            flex-direction: column;
            align-items: center;
            text-align: center;
            padding: 0 60px;
            box-sizing: border-box;
        }

        .brand-logo {
            font-size: 32px;
            font-weight: 700;
            color: var(--text-white);
            text-transform: uppercase;
            letter-spacing: 4px;
            margin-bottom: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .divider {
            width: 100%;
            height: 4px;
            background-color: var(--text-white);
            margin-bottom: 60px;
        }

        .main-text {
            color: var(--text-white);
            font-size: 88px;
            /* Extra bold and large */
            font-weight: 900;
            line-height: 1.1;
            text-transform: uppercase;
            letter-spacing: -2px;
        }

        .highlight {
            display: block;
            margin-bottom: 10px;
        }
    </style>
</head>

<body>

    <div class="canvas">
        <div class="content-container">
            <div class="brand-logo">
                <span style="font-size: 18px; letter-spacing: 8px;">FORESEEN</span>
                HEALTHCARE
            </div>

            <div class="divider"></div>

            <div class="main-text">
                <span class="highlight">Eat garlic every day</span>
                <span class="highlight">for 30 days.</span>
                <span class="highlight" style="font-weight: 400; opacity: 0.9;">Your breath will suffer.</span>
                <span class="highlight">Your body will evolve.</span>
            </div>
        </div>
    </div>

</body>

</html>