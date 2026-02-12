<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Web AR - Reacción Química con Marcadores</title>
    <script src="https://aframe.io/releases/1.4.0/aframe.min.js"></script>
    <script src="https://raw.githack.com/AR-js-org/AR.js/master/aframe/build/aframe-ar.js"></script>
    <style>
        body { 
            margin: 0; 
            overflow: hidden; 
            font-family: 'Arial', sans-serif; 
        }

        /* Panel de información detallada */
        #info-panel {
            position: absolute;
            bottom: 30px;
            left: 30px;
            width: 280px;
            background: rgba(10, 20, 30, 0.85);
            backdrop-filter: blur(5px);
            border-left: 6px solid #00ccff;
            border-radius: 12px;
            padding: 18px 22px;
            color: white;
            font-size: 16px;
            box-shadow: 0 8px 20px rgba(0,0,0,0.5);
            border: 1px solid rgba(255,255,255,0.2);
            transition: opacity 0.3s ease;
            z-index: 100;
        }
        #info-panel h3 {
            margin: 0 0 10px 0;
            color: #88ddff;
            font-weight: 600;
            font-size: 20px;
            border-bottom: 1px solid #4488aa;
            padding-bottom: 6px;
        }
        #info-panel p {
            margin: 8px 0;
            line-height: 1.5;
        }
        .highlight {
            color: #ffaa66;
            font-weight: bold;
        }
        .hidden-panel {
            opacity: 0;
            visibility: hidden;
        }
        .visible-panel {
            opacity: 1;
            visibility: visible;
        }

        /* Instrucciones */
        #instructions {
            position: absolute;
            top: 30px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0,0,0,0.8);
            color: white;
            padding: 15px 25px;
            border-radius: 15px;
            font-size: 16px;
            border: 2px solid #00ccff;
            backdrop-filter: blur(2px);
            z-index: 200;
            text-align: center;
            max-width: 80%;
        }

        /* Botón toggle info */
        #toggle-info {
            position: absolute;
            top: 30px;
            right: 30px;
            background: rgba(0,150,255,0.9);
            color: white;
            padding: 12px 20px;
            border-radius: 25px;
            font-size: 14px;
            border: 2px solid white;
            cursor: pointer;
            z-index: 200;
            box-shadow: 0 4px 10px rgba(0,0,0,0.3);
        }
        #toggle-info:hover {
            background: rgba(0,180,255,1);
        }

        .ar-label {
            color: white;
            font-size: 0.3;
            text-align: center;
            background: rgba(0,0,0,0.7);
            padding: 0.1 0.2;
            border-radius: 0.1;
        }
    </style>
</head>
<body>
    <!-- Panel de información -->
    <div id="info-panel" class="visible-panel">
        <h3 id="particle-title">Información Química</h3>
        <p><span class="highlight">Marcador Hiro:</span> Oxígeno (O)</p>
        <p><span class="highlight">Marcador Kanji:</span> Hidrógeno (H₂)</p>
        <p><span class="highlight">Ambos juntos:</span> Agua (H₂O)</p>
        <p style="margin-top:12px; font-style:italic; color:#aaa;" id="extra-info">
            Acerca ambos marcadores para ver la reacción química.
        </p>
    </div>

    <!-- Instrucciones -->
    <div id="instructions">
        📱 Apunta la cámara a los marcadores Hiro y Kanji<br>
        <small>Acerca los marcadores para fusionar las moléculas</small>
    </div>

    <!-- Botón toggle -->
    <button id="toggle-info" onclick="toggleInfo()">ℹ️ Info</button>

    <!-- Escena A-Frame con AR.js -->
    <a-scene 
        embedded 
        arjs="sourceType: webcam; debugUIEnabled: false; detectionMode: mono_and_matrix; matrixCodeType: 3x3;"
        vr-mode-ui="enabled: false"
        renderer="logarithmicDepthBuffer: true; precision: medium;">
        
        <a-assets>
            <!-- Assets si se necesitan -->
        </a-assets>

        <!-- Marcador Hiro - Oxígeno -->
        <a-marker id="marker-hiro" preset="hiro" emitevents="true">
            <!-- Átomo de Oxígeno -->
            <a-entity id="oxygen-atom">
                <!-- Núcleo -->
                <a-sphere 
                    position="0 0 0" 
                    radius="0.16" 
                    color="#ff5555"
                    metalness="0.3"
                    roughness="0.25">
                </a-sphere>
                
                <!-- Órbita 1 -->
                <a-torus 
                    position="0 0 0" 
                    radius="0.28" 
                    radius-tubular="0.025"
                    color="#88ccff"
                    rotation="90 0 17"
                    opacity="0.5"
                    transparent="true">
                </a-torus>
                
                <!-- Órbita 2 -->
                <a-torus 
                    position="0 0 0" 
                    radius="0.31" 
                    radius-tubular="0.025"
                    color="#88ccff"
                    rotation="0 90 29"
                    opacity="0.5"
                    transparent="true">
                </a-torus>
                
                <!-- Etiqueta -->
                <a-text 
                    value="Oxigeno (O)" 
                    position="0 0.4 0" 
                    align="center"
                    color="white"
                    width="1.5"
                    class="ar-label">
                </a-text>
            </a-entity>
        </a-marker>

        <!-- Marcador Kanji - Hidrógeno H2 -->
        <a-marker id="marker-kanji" preset="kanji" emitevents="true">
            <!-- Molécula H2 -->
            <a-entity id="h2-molecule">
                <!-- Átomo H1 -->
                <a-sphere 
                    position="-0.15 0 0" 
                    radius="0.1" 
                    color="#ffffff"
                    metalness="0.1"
                    roughness="0.3">
                </a-sphere>
                
                <!-- Átomo H2 -->
                <a-sphere 
                    position="0.15 0 0" 
                    radius="0.1" 
                    color="#ffffff"
                    metalness="0.1"
                    roughness="0.3">
                </a-sphere>
                
                <!-- Enlace -->
                <a-cylinder 
                    position="0 0 0" 
                    radius="0.03" 
                    height="0.3"
                    color="#aaaaaa"
                    rotation="0 0 90"
                    metalness="0.8"
                    roughness="0.6">
                </a-cylinder>
                
                <!-- Etiqueta -->
                <a-text 
                    value="Hidrogeno (H2)" 
                    position="0 0.35 0" 
                    align="center"
                    color="#aaddff"
                    width="1.5">
                </a-text>
            </a-entity>
        </a-marker>

        <!-- Molécula de Agua (se mostrará cuando ambos marcadores estén cerca) -->
        <a-entity id="water-molecule" visible="false">
            <!-- Oxígeno central -->
            <a-sphere 
                position="0 0 0" 
                radius="0.16" 
                color="#ff3333"
                metalness="0.2"
                roughness="0.25">
            </a-sphere>
            
            <!-- Hidrógeno 1 -->
            <a-sphere 
                position="0.2 0.15 0" 
                radius="0.1" 
                color="#ffffff"
                metalness="0.1"
                roughness="0.3">
            </a-sphere>
            
            <!-- Hidrógeno 2 -->
            <a-sphere 
                position="0.2 -0.15 0" 
                radius="0.1" 
                color="#ffffff"
                metalness="0.1"
                roughness="0.3">
            </a-sphere>
            
            <!-- Enlace 1 -->
            <a-cylinder 
                position="0.1 0.075 0" 
                radius="0.035" 
                height="0.25"
                color="#cccccc"
                rotation="0 0 -40"
                metalness="0.8"
                roughness="0.5">
            </a-cylinder>
            
            <!-- Enlace 2 -->
            <a-cylinder 
                position="0.1 -0.075 0" 
                radius="0.035" 
                height="0.25"
                color="#cccccc"
                rotation="0 0 40"
                metalness="0.8"
                roughness="0.5">
            </a-cylinder>
            
            <!-- Etiqueta -->
            <a-text 
                value="Agua (H2O)" 
                position="0 0.4 0" 
                align="center"
                color="#88ddff"
                width="1.5">
            </a-text>
        </a-entity>

        <a-entity camera></a-entity>
    </a-scene>

    <script>
        let audioContext = null;
        let isFused = false;
        const FUSION_DISTANCE = 0.8; // Distancia en metros para fusión

        // Referencias a los marcadores
        const hiroMarker = document.querySelector('#marker-hiro');
        const kanjiMarker = document.querySelector('#marker-kanji');
        const waterMolecule = document.querySelector('#water-molecule');
        const oxygenAtom = document.querySelector('#oxygen-atom');
        const h2Molecule = document.querySelector('#h2-molecule');
        const infoPanel = document.querySelector('#info-panel');

        // Variables de estado
        let hiroVisible = false;
        let kanjiVisible = false;
        let hiroPosition = null;
        let kanjiPosition = null;

        // Sonido de fusión
        function playCollisionSound() {
            try {
                if (!audioContext) audioContext = new (window.AudioContext || window.webkitAudioContext)();
                if (audioContext.state === 'suspended') audioContext.resume();
                const oscillator = audioContext.createOscillator();
                const gainNode = audioContext.createGain();
                oscillator.type = 'sine';
                oscillator.frequency.value = 660;
                gainNode.gain.setValueAtTime(0.25, audioContext.currentTime);
                gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.15);
                oscillator.connect(gainNode);
                gainNode.connect(audioContext.destination);
                oscillator.start();
                oscillator.stop(audioContext.currentTime + 0.15);
            } catch(e) { 
                console.log('Audio error:', e); 
            }
        }

        // Actualizar panel de información
        function updateInfoPanel(molecule) {
            const title = document.getElementById('particle-title');
            const info = document.getElementById('extra-info');
            
            if (molecule === 'water') {
                title.innerText = 'Agua (H₂O)';
                info.innerText = '¡Reacción completa! Molécula polar, disolvente universal. Masa: 18.015 u';
            } else if (molecule === 'oxygen') {
                title.innerText = 'Oxígeno (O)';
                info.innerText = 'Gas esencial para la vida. Número atómico: 8. Electronegatividad: 3.44';
            } else if (molecule === 'hydrogen') {
                title.innerText = 'Hidrógeno (H₂)';
                info.innerText = 'Elemento más abundante del universo. Número atómico: 1. Masa: 1.008 u';
            } else if (molecule === 'both') {
                title.innerText = 'Acerca los marcadores';
                info.innerText = 'Los dos reactivos están presentes. ¡Acércalos para formar agua!';
            }
        }

        // Eventos de marcadores
        hiroMarker.addEventListener('markerFound', () => {
            console.log('Hiro marker found');
            hiroVisible = true;
            if (!kanjiVisible) {
                updateInfoPanel('oxygen');
            } else {
                updateInfoPanel('both');
            }
        });

        hiroMarker.addEventListener('markerLost', () => {
            console.log('Hiro marker lost');
            hiroVisible = false;
            if (kanjiVisible) {
                updateInfoPanel('hydrogen');
            }
        });

        kanjiMarker.addEventListener('markerFound', () => {
            console.log('Kanji marker found');
            kanjiVisible = true;
            if (!hiroVisible) {
                updateInfoPanel('hydrogen');
            } else {
                updateInfoPanel('both');
            }
        });

        kanjiMarker.addEventListener('markerLost', () => {
            console.log('Kanji marker lost');
            kanjiVisible = false;
            if (hiroVisible) {
                updateInfoPanel('oxygen');
            }
        });

        // Loop de animación
        function checkFusion() {
            if (hiroVisible && kanjiVisible) {
                // Obtener posiciones mundiales
                const hiroPos = new THREE.Vector3();
                const kanjiPos = new THREE.Vector3();
                
                hiroMarker.object3D.getWorldPosition(hiroPos);
                kanjiMarker.object3D.getWorldPosition(kanjiPos);
                
                const distance = hiroPos.distanceTo(kanjiPos);
                
                // Si están cerca, fusionar
                if (distance < FUSION_DISTANCE) {
                    if (!isFused) {
                        isFused = true;
                        playCollisionSound();
                        updateInfoPanel('water');
                    }
                    
                    // Ocultar moléculas individuales
                    oxygenAtom.setAttribute('visible', false);
                    h2Molecule.setAttribute('visible', false);
                    
                    // Mostrar agua en punto medio
                    const midPoint = new THREE.Vector3().addVectors(hiroPos, kanjiPos).multiplyScalar(0.5);
                    waterMolecule.object3D.position.copy(midPoint);
                    waterMolecule.setAttribute('visible', true);
                } else {
                    if (isFused) {
                        isFused = false;
                        updateInfoPanel('both');
                    }
                    
                    // Mostrar moléculas individuales
                    oxygenAtom.setAttribute('visible', true);
                    h2Molecule.setAttribute('visible', true);
                    waterMolecule.setAttribute('visible', false);
                }
            } else {
                // Si no están ambos visibles, resetear
                if (isFused) {
                    isFused = false;
                }
                waterMolecule.setAttribute('visible', false);
                if (hiroVisible) oxygenAtom.setAttribute('visible', true);
                if (kanjiVisible) h2Molecule.setAttribute('visible', true);
            }
            
            requestAnimationFrame(checkFusion);
        }

        // Iniciar loop
        checkFusion();

        // Toggle info panel
        function toggleInfo() {
            if (infoPanel.classList.contains('visible-panel')) {
                infoPanel.classList.remove('visible-panel');
                infoPanel.classList.add('hidden-panel');
            } else {
                infoPanel.classList.remove('hidden-panel');
                infoPanel.classList.add('visible-panel');
            }
        }

        // Activar audio en primer click
        document.body.addEventListener('click', () => {
            if (audioContext && audioContext.state === 'suspended') {
                audioContext.resume();
            }
        }, { once: true });
    </script>
</body>
</html>
