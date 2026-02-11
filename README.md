# index.html
@ -1,2 +1,89 @@
# Auto detect text files and perform LF normalization
* text=auto
import ipywidgets as widgets
from IPython.display import display, HTML, clear_output

# 1. Création des champs de saisie
nom_input = widgets.Text(placeholder="Ton nom ici...", description="Nom :")
age_input = widgets.IntText(value=0, description="Âge :")
bouton = widgets.Button(description="Clique pour la surprise ! 🎁", button_style='success')

def afficher_surprise(b):
    nom = nom_input.value
    age = age_input.value

    # Nettoie la cellule pour laisser place au spectacle
    clear_output()

    html_code = f"""
    <div id="container" style="background: radial-gradient(circle, #1a1a2e 0%, #000 100%); height: 500px; width: 100%; position: relative; overflow: hidden; border-radius: 20px; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;">
        <div style="position: absolute; width: 100%; top: 40%; z-index: 10; text-align: center; pointer-events: none;">
            <h1 style="color: #ff4d4d; font-size: 3em; margin: 0; text-shadow: 0 0 10px #ff4d4d, 0 0 20px #ff4d4d;">✨ JOYEUX ANNIVERSAIRE .
            Tu as toujours été là pour moi quand j'en avais besoin et je ne l'oublierai pas .
            Je suis très reconnaissante de t'avoir dans ma vie.
             Je t'aime de tout mon coeur  ✨</h1>
            <h2 style="color: white; font-size: 2em; margin: 10px 0;">{nom.upper()}</h2>
            <p style="color: gold; font-size: 1.5em; letter-spacing: 2px;">🎊 {age} ANS DE BONHEUR 🎊</p>
        </div>
        <canvas id="fireworksCanvas"></canvas>
    </div>

    <script>
    (function() {{
        const canvas = document.getElementById('fireworksCanvas');
        const ctx = canvas.getContext('2d');
        const container = document.getElementById('container');
        canvas.width = container.offsetWidth;
        canvas.height = container.offsetHeight;

        let particles = [];
        const colors = ['#FF1493', '#00BFFF', '#FFD700', '#ADFF2F', '#FF4500', '#FFFFFF'];

        class Particle {{
            constructor(x, y, color) {{
                this.x = x; this.y = y; this.color = color;
                this.velocity = {{ x: (Math.random() - 0.5) * 12, y: (Math.random() - 0.5) * 12 }};
                this.alpha = 1; this.friction = 0.96;
            }}
            draw() {{
                ctx.globalAlpha = this.alpha;
                ctx.beginPath();
                ctx.arc(this.x, this.y, 2.5, 0, Math.PI * 2);
                ctx.fillStyle = this.color;
                ctx.fill();
            }}
            update() {{
                this.velocity.x *= this.friction;
                this.velocity.y *= this.friction;
                this.y += 0.15; // Gravité
                this.x += this.velocity.x;
                this.y += this.velocity.y;
                this.alpha -= 0.012;
            }}
        }}

        function createExplosion(x, y) {{
            const color = colors[Math.floor(Math.random() * colors.length)];
            for (let i = 0; i < 40; i++) particles.push(new Particle(x, y, color));
        }}

        function animate() {{
            requestAnimationFrame(animate);
            ctx.fillStyle = 'rgba(0, 0, 0, 0.15)';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            particles.forEach((p, i) => {{
                if (p.alpha > 0) {{ p.update(); p.draw(); }}
                else {{ particles.splice(i, 1); }}
            }});
            if (Math.random() < 0.08) createExplosion(Math.random() * canvas.width, Math.random() * canvas.height);
        }}
        animate();
    }})();
    </script>
    """
    display(HTML(html_code))

# 2. Affichage du formulaire au début
print("--- CONFIGURATION DE LA SURPRISE ---")
bouton.on_click(afficher_surprise)
display(nom_input, age_input, bouton)
