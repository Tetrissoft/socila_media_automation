You are AI ASSISTANT. Your task is to generate HTML/CSS based on the provided "Layout Type" and "Visual Theme" from the Content Strategist.

**Input:**
- Visual Theme: `{{ $('Content Strategist').item.json.visual_theme }}`
- Layout Type: `{{ $('Content Strategist').item.json.layout_type }}`
- Headline: `{{ $('Content Strategist').item.json.headline }}`
- Sub-text/Points: `{{ $('Content Strategist').item.json.sub_text }}`

**Instructions:**
1.  Analyze the `layout_type`.
2.  Select the corresponding CSS class and HTML structure.

---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Bake Me A Wish Dynamic</title>
    <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;500;800&family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=Permanent+Marker&family=Abril+Fatface&display=swap" rel="stylesheet">
    <style>
        /* BASE & RESET */
        body { margin:0; padding:0; -webkit-font-smoothing: antialiased; }
        .canvas { width: 1080px; height: 1350px; position: relative; overflow: hidden; display: flex; flex-direction: column; box-sizing: border-box; }
        
        /* === VISUAL THEMES (SKINS) === */
        
        /* 1. Classic Rustic */
        .Classic_Rustic {
            --bg-color: #f4e4d4;
            --text-color: #5d4037;
            --accent-color: #8d6e63;
            --font-head: 'Playfair Display', serif;
            --font-body: 'Outfit', sans-serif;
            --texture: url('https://www.transparenttextures.com/patterns/wood-pattern.png'); /* Placeholder texture */
            --border-radius: 4px;
        }
        
        /* 2. Modern Pop */
        .Modern_Pop {
            --bg-color: #ffe082;
            --text-color: #212121;
            --accent-color: #ff5252;
            --font-head: 'Outfit', sans-serif;
            --font-body: 'Outfit', sans-serif;
            --texture: radial-gradient(circle, transparent 20%, #ffecb3 20%, #ffecb3 80%, transparent 80%, transparent) 0% 0% / 40px 40px;
            --border-radius: 0px;
        }

        /* 3. Elegant Luxury */
        .Elegant_Luxury {
            --bg-color: #212121;
            --text-color: #fafafa;
            --accent-color: #d4af37; /* Gold */
            --font-head: 'Abril Fatface', cursive;
            --font-body: 'Playfair Display', serif;
            --texture: linear-gradient(45deg, #2c2c2c 25%, transparent 25%, transparent 75%, #2c2c2c 75%, #2c2c2c), linear-gradient(45deg, #2c2c2c 25%, transparent 25%, transparent 75%, #2c2c2c 75%, #2c2c2c);
            --border-radius: 20px;
        }

        /* Apply Theme Vars */
        .canvas {
            background-color: var(--bg-color);
            background-image: var(--texture);
            color: var(--text-color);
            font-family: var(--font-body);
        }
        h1 { font-family: var(--font-head); margin: 0; }
        
        /* === CONTENT LAYOUTS === */

        /* A. COMPARISON (VS) */
        .VS_Split_Classic { flex-direction: row; }
        .VS_Split_Classic .side { flex: 1; display: flex; align-items: center; justify-content: center; position: relative; padding: 40px; }
        .VS_Split_Classic .side:first-child { background: rgba(0,0,0,0.05); }
        .VS_Split_Classic .vs-badge { position: absolute; left: 50%; top: 50%; transform: translate(-50%, -50%); background: var(--accent-color); color: white; padding: 20px; border-radius: 50%; font-weight: bold; z-index: 10; font-size: 30px;}

        .VS_Diagonal { position: relative; }
        .VS_Diagonal .side { position: absolute; width: 100%; height: 100%; clip-path: polygon(0 0, 100% 0, 0 100%); background: var(--bg-color); }
        .VS_Diagonal .side:nth-child(2) { clip-path: polygon(100% 0, 100% 100%, 0 100%); background: var(--accent-color); color: white; }
        .VS_Diagonal .content-wrap { position: absolute; padding: 60px; }
        .VS_Diagonal .side:nth-child(1) .content-wrap { top: 0; left: 0; width: 50%; }
        .VS_Diagonal .side:nth-child(2) .content-wrap { bottom: 0; right: 0; width: 50%; text-align: right; }

        .VS_Cards_Overlap { align-items: center; justify-content: center; background-image: url('PLACEHOLDER_BG'); background-size: cover;}
        .VS_Cards_Overlap .card { width: 600px; height: 500px; background: var(--bg-color); box-shadow: 0 20px 50px rgba(0,0,0,0.3); border-radius: var(--border-radius); position: absolute; padding: 40px; display: flex; flex-direction: column; justify-content: center;}
        .VS_Cards_Overlap .card:nth-child(1) { top: 150px; left: 100px; transform: rotate(-5deg); z-index: 1; }
        .VS_Cards_Overlap .card:nth-child(2) { bottom: 150px; right: 100px; transform: rotate(5deg); z-index: 2; background: white; color: black;}

        /* B. LISTS */
        .List_Simple_Bullets { padding: 80px; }
        .List_Simple_Bullets h1 { font-size: 80px; margin-bottom: 60px; color: var(--accent-color); }
        .List_Simple_Bullets ul { list-style: none; padding: 0; }
        .List_Simple_Bullets li { font-size: 40px; margin-bottom: 30px; display: flex; align-items: start; }
        .List_Simple_Bullets li::before { content: '✓'; color: var(--accent-color); margin-right: 20px; font-weight: bold; }

        .Steps_Timeline { padding: 80px; display: flex; flex-direction: column; justify-content: center; }
        .Steps_Timeline .step { display: flex; align-items: center; margin-bottom: 60px; position: relative; }
        .Steps_Timeline .step-num { font-size: 60px; font-weight: bold; color: var(--accent-color); margin-right: 40px; min-width: 80px; }
        .Steps_Timeline .step-content { font-size: 35px; background: rgba(255,255,255,0.5); padding: 30px; border-radius: var(--border-radius); flex: 1; }
        
        .Grid_Features { display: grid; grid-template-columns: 1fr 1fr; grid-template-rows: auto 1fr 1fr; gap: 40px; padding: 60px; }
        .Grid_Features h1 { grid-column: span 2; text-align: center; font-size: 70px; }
        .Grid_Features .grid-item { background: rgba(255,255,255,0.1); border: 2px solid var(--accent-color); padding: 40px; border-radius: var(--border-radius); display: flex; align-items: center; justify-content: center; text-align: center; font-size: 30px; font-weight: bold; }

        /* C. SHOWCASE */
        .Hero_Full_Focus { background-image: url('PLACEHOLDER_BG'); background-size: cover; justify-content: flex-end; }
        .Hero_Full_Focus .overlay { background: linear-gradient(to top, rgba(0,0,0,0.9), transparent); padding: 80px; color: white; }
        .Hero_Full_Focus h1 { font-size: 90px; margin-bottom: 20px; }

        .Product_Badge { align-items: center; justify-content: center; background-image: url('PLACEHOLDER_BG'); background-size: cover; }
        .Product_Badge .badge { width: 700px; height: 700px; background: rgba(255,255,255,0.9); border-radius: 50%; display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center; padding: 60px; box-shadow: 0 0 50px rgba(0,0,0,0.5); border: 10px solid var(--accent-color); }
        .Product_Badge h1 { font-size: 70px; color: var(--accent-color); line-height: 1.1; }

        /* D. TEXT & QUOTES */
        .Quote_Block_Bold { justify-content: center; align-items: center; padding: 100px; text-align: center; background: var(--accent-color); color: white; }
        .Quote_Block_Bold h1 { font-size: 100px; line-height: 1; text-transform: uppercase; }
        .Quote_Block_Bold .author { margin-top: 40px; font-size: 40px; font-style: italic; opacity: 0.8; }

        .Typography_Neon { background: #111; color: #fff; justify-content: center; align-items: center; padding: 80px; text-align: center; }
        .Typography_Neon h1 { font-size: 110px; color: #fff; text-shadow: 0 0 10px var(--accent-color), 0 0 20px var(--accent-color), 0 0 40px var(--accent-color); font-family: 'Permanent Marker', cursive; }

        /* E. ENGAGEMENT */
        .Poll_Question { padding: 80px; justify-content: center; }
        .Poll_Question h1 { text-align: center; font-size: 70px; margin-bottom: 80px; }
        .Poll_Question .option { background: white; padding: 40px 60px; border-radius: 50px; margin-bottom: 40px; font-size: 40px; display: flex; justify-content: space-between; border: 3px solid transparent; box-shadow: 0 10px 20px rgba(0,0,0,0.1); color: #333; }
        .Poll_Question .option:hover { border-color: var(--accent-color); transform: scale(1.02); }

        .This_Or_That_Stack { flex-direction: column; }
        .This_Or_That_Stack .top { flex: 1; background: var(--bg-color); display: flex; align-items: center; justify-content: center; font-size: 60px; font-weight: bold; border-bottom: 5px solid white; }
        .This_Or_That_Stack .bottom { flex: 1; background: var(--accent-color); color: white; display: flex; align-items: center; justify-content: center; font-size: 60px; font-weight: bold; }
        .This_Or_That_Stack .or-badge { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); background: white; color: black; padding: 20px 40px; border-radius: 40px; font-weight: 900; font-size: 40px; border: 5px solid var(--accent-color); }

        /* F. ANNOUNCEMENTS */
        .Breaking_News_Banner { justify-content: center; align-items: center; background: yellow; color: black; }
        .Breaking_News_Banner h1 { font-size: 120px; text-transform: uppercase; border: 10px solid black; padding: 40px; transform: rotate(-5deg); background: white; box-shadow: 20px 20px 0px black; }

        .Event_Ticket_Stub { justify-content: center; align-items: center; background: #333; }
        .Event_Ticket_Stub .ticket { width: 800px; height: 1000px; background: var(--bg-color); position: relative; border-radius: 20px; overflow: hidden; display: flex; flex-direction: column; }
        .Event_Ticket_Stub .ticket-header { background: var(--accent-color); color: white; padding: 40px; text-align: center; font-size: 50px; font-weight: bold; border-bottom: 2px dashed rgba(255,255,255,0.5); }
        .Event_Ticket_Stub .ticket-body { padding: 60px; flex: 1; display: flex; flex-direction: column; justify-content: center; align-items: center; text-align: center; }
        .Event_Ticket_Stub h1 { font-size: 80px; margin-bottom: 30px; color: var(--text-color); }
        
    </style>
</head>
<body>
    <div class="canvas {{ $('Content Strategist').item.json.visual_theme.replace(/ /g, '_') }} {{ $('Content Strategist').item.json.layout_type }}">
        
        <!-- GENERIC CONTENT INJECTION (Structure depends on CSS classes mostly, but we can do simple logic) -->
        
        <!-- Standard Header usually works for most -->
        <!-- Logic to handle different structures if needed -->
        
        <div class="content-wrapper">
             {% if $('Content Strategist').item.json.layout_type.includes('VS') %}
                <!-- VS Structure -->
                 <div class="side">
                    <div class="content-wrap">
                        <h1>{{ $('Content Strategist').item.json.sub_text.split('|')[0] || 'Option A' }}</h1>
                    </div>
                 </div>
                 <div class="vs-badge">VS</div>
                 <div class="side">
                     <div class="content-wrap">
                        <h1>{{ $('Content Strategist').item.json.sub_text.split('|')[1] || 'Option B' }}</h1>
                     </div>
                 </div>

             {% elseif $('Content Strategist').item.json.layout_type.includes('List') %}
                <!-- List Structure -->
                <h1>{{ $('Content Strategist').item.json.headline }}</h1>
                <ul>
                    {% set points = $('Content Strategist').item.json.sub_text.split('|') %}
                    {% for point in points %}
                        <li>{{ point }}</li>
                    {% endfor %}
                </ul>

             {% elseif $('Content Strategist').item.json.layout_type.includes('Quote') %}
                 <!-- Quote Structure -->
                 <h1>"{{ $('Content Strategist').item.json.sub_text }}"</h1>
                 <div class="author">- Bake Me A Wish</div>

             {% else %}
                <!-- Default/Fallback Structure -->
                <h1>{{ $('Content Strategist').item.json.headline }}</h1>
                <h3>{{ $('Content Strategist').item.json.sub_text }}</h3>
             {% endif %}
        </div>
        
        <!-- Branding Footer -->
        <div style="position: absolute; bottom: 40px; left: 50%; transform: translateX(-50%); font-size: 20px; opacity: 0.7; letter-spacing: 2px;">BAKE ME A WISH</div>
    </div>
</body>
</html>
