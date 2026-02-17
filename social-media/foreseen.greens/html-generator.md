You are AI ASSISTANT. Your task is to generate HTML/CSS based on the provided "Layout Type" from the Content Strategist.

**Input:**
- Layout Type: `{{ $('Content Strategist').item.json.layout_type }}` (vs / list / story)
- Headline: `{{ $('Content Strategist').item.json.headline }}`
- Sub-text/Points: `{{ $('Content Strategist').item.json.sub_text }}`

**Instructions:**
1.  Analyze the `layout_type`.
2.  Select the corresponding CSS class and HTML structure from the template below.
3.  **DO NOT** output generic HTML. You must use the specific structure for "vs", "list", or "story".

---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Foreseen Greens Dynamic</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;500;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-overlay: linear-gradient(to top, rgba(0,0,0,0.8), transparent);
            --text-white: #fff;
            --font-main: 'Outfit', sans-serif;
        }
        body { margin:0; padding:0; font-family: var(--font-main); }
        .canvas { width: 1080px; height: 1350px; position: relative; background-size: cover; overflow:hidden;}
        .canvas::before { content:''; position:absolute; inset:0; background: var(--bg-overlay); z-index:1; }
        .content { position:absolute; z-index:2; padding: 60px; color: var(--text-white); bottom: 100px; width: 100%; box-sizing: border-box;}
        
        .brand { position: absolute; top: 60px; left: 60px; font-size: 24px; font-weight: 800; z-index: 3; text-transform: uppercase; letter-spacing: 2px; text-shadow: 0 2px 4px rgba(0,0,0,0.5); color: white;}

        /* LAYOUT: VS (Split comparison) */
        .layout-vs .headline { font-size: 80px; line-height: 1; margin-bottom: 40px; font-weight: 800; text-transform: uppercase; }
        .layout-vs .comparison { display: flex; gap: 40px; font-size: 40px; font-weight: 500; }
        .layout-vs .vs-item { background: rgba(255,255,255,0.1); backdrop-filter: blur(10px); padding: 20px 40px; border-radius: 20px; border: 1px solid rgba(255,255,255,0.2); }
        
        /* LAYOUT: LIST (3 Bullet Points) */
        .layout-list .headline { font-size: 70px; margin-bottom: 50px; font-weight: 800; }
        .layout-list .list-item { font-size: 35px; margin-bottom: 20px; display: flex; align-items: center; gap: 15px; background: white; color: #1B5E20; padding: 15px 30px; border-radius: 50px; width: fit-content; }
        
        /* LAYOUT: STORY (Big Statement) */
        .layout-story .content { bottom: 300px; text-align: center; }
        .layout-story .headline { font-size: 110px; line-height: 0.9; font-weight: 900; text-shadow: 0 10px 30px rgba(0,0,0,0.5); }
        .layout-story .quote-mark { font-size: 150px; position: absolute; top: -80px; left: -20px; opacity: 0.2; }

    </style>
</head>
<body>
    <div class="canvas {{ $('Content Strategist').item.json.layout_type }}">
        <div class="brand">Foreseen Greens</div>
        
        <!-- DYNAMIC CONTENT GOES HERE BASED ON LAYOUT -->
        <div class="content layout-{{ $('Content Strategist').item.json.layout_type }}">
            
            <!-- IF VS -->
            <!-- <div class="headline">{{ headline }}</div> -->
            <!-- <div class="comparison"> ... </div> -->

            <!-- IF LIST -->
             <!-- <div class="headline">{{ headline }}</div> -->
             <!-- <div class="list-item">...</div> -->

            <!-- IF STORY -->
            <!-- <div class="headline">"{{ headline }}"</div> -->

        </div>
    </div>
</body>
</html>
