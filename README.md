<!doctype html>
<html lang="en">
<head>
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width, initial-scale=1">
	<title>Bytebeat Atelier</title>
	<style>
		:root {
			--bg: #111312;
			--panel: #191c1a;
			--panel-2: #212622;
			--line: #353c36;
			--text: #f1f2e9;
			--muted: #9da79c;
			--accent: #d6f36a;
			--accent-2: #65d7bb;
			--danger: #ff806e;
			--shadow: 0 20px 60px rgba(0, 0, 0, .24);
		}
		[data-theme="cobalt"] { --bg:#101622; --panel:#172131; --panel-2:#1d2c40; --line:#34465b; --text:#edf5ff; --muted:#9eb1c7; --accent:#81b8ff; --accent-2:#78e7d3; }
		[data-theme="sunset"] { --bg:#24191a; --panel:#30201f; --panel-2:#402827; --line:#61403a; --text:#fff2df; --muted:#c9a797; --accent:#ffc36b; --accent-2:#ff928d; }
		[data-theme="mono"] { --bg:#111; --panel:#1a1a1a; --panel-2:#232323; --line:#3a3a3a; --text:#f3f3f3; --muted:#aaa; --accent:#fff; --accent-2:#bbb; }
		* { box-sizing:border-box; }
		body { margin:0; min-width:320px; color:var(--text); background:var(--bg); font-family: Georgia, 'Times New Roman', serif; }
		body:before { content:""; position:fixed; inset:0; pointer-events:none; opacity:.22; background-image:linear-gradient(rgba(255,255,255,.025) 1px, transparent 1px),linear-gradient(90deg, rgba(255,255,255,.025) 1px, transparent 1px); background-size:32px 32px; }
		.app { position:relative; max-width:1600px; margin:auto; padding:10px 12px 32px; }
		header { display:flex; align-items:center; gap:10px; margin-bottom:10px; padding:7px 8px; border:1px solid var(--line); background:linear-gradient(#2a302c, #171b19); box-shadow:0 5px 18px rgba(0,0,0,.35); }
		.brand { display:flex; align-items:center; gap:8px; min-width:135px; }
		.kicker { margin:0; color:var(--accent); font:700 10px ui-monospace, SFMono-Regular, Consolas, monospace; letter-spacing:.12em; text-transform:uppercase; }
		h1 { margin:0; color:var(--text); font:700 16px ui-monospace, SFMono-Regular, Consolas, monospace; letter-spacing:0; }
		.subtitle { display:none; }
		.instrument-bar { display:flex; align-items:center; gap:5px; min-width:0; flex:1; }
		.readout { display:flex; align-items:center; gap:8px; min-width:102px; padding:7px 10px; border:1px solid #4b554d; background:#0e110f; color:var(--accent); font:700 13px ui-monospace, SFMono-Regular, Consolas, monospace; }
		.readout b { color:var(--muted); font-weight:400; }
		.layout { display:flex; flex-direction:column; gap:10px; }
		.panel { border:1px solid var(--line); background:linear-gradient(145deg, var(--panel), color-mix(in srgb, var(--panel) 83%, #000)); box-shadow:var(--shadow); }
		.editor-panel { min-height:0; display:flex; flex-direction:column; }
		.panel-head { display:flex; justify-content:space-between; align-items:center; gap:12px; min-height:58px; padding:15px 18px; border-bottom:1px solid var(--line); }
		.panel-title { margin:0; color:var(--muted); font:700 11px ui-monospace, SFMono-Regular, Consolas, monospace; text-transform:uppercase; letter-spacing:.16em; }
		.controls, .toolbar { display:flex; align-items:center; flex-wrap:wrap; gap:8px; }
		button, select, input { font:inherit; }
		button, select { color:var(--text); border:1px solid var(--line); background:var(--panel-2); padding:9px 12px; cursor:pointer; }
		button:hover, select:hover { border-color:var(--accent); }
		button.primary { color:#10150a; background:var(--accent); border-color:var(--accent); font-weight:700; }
		button.primary:hover { filter:brightness(1.08); }
		button.download { color:#08130f; background:var(--accent-2); border-color:var(--accent-2); font-weight:700; }
		button.icon { width:40px; padding:9px 0; font-size:16px; }
		.editor-wrap { position:relative; flex:1; min-height:340px; background:#0e100f; }
		textarea { display:block; width:100%; height:100%; min-height:340px; padding:25px 25px 25px 64px; resize:none; outline:none; border:0; color:var(--accent); background:transparent; font:clamp(18px, 2.6vw, 31px)/1.55 ui-monospace, SFMono-Regular, Consolas, monospace; tab-size:2; }
		.line-no { position:absolute; left:0; top:0; bottom:0; width:47px; padding:29px 10px; border-right:1px solid #242a25; color:#566256; text-align:right; white-space:pre; font:16px/1.55 ui-monospace, SFMono-Regular, Consolas, monospace; pointer-events:none; }
		.editor-foot { display:flex; justify-content:space-between; gap:12px; padding:12px 18px; border-top:1px solid var(--line); color:var(--muted); font:11px ui-monospace, SFMono-Regular, Consolas, monospace; }
		.right-col { display:flex; flex-direction:column; gap:10px; order:1; }
		.editor-panel { order:2; }
		.scope { width:100%; aspect-ratio:3.25; display:block; background:#0e100f; }
		.scope-wrap { padding:10px 12px 12px; }
		.graph-tabs { display:flex; gap:5px; margin-bottom:10px; }
		.graph-tabs button { padding:7px 10px; color:var(--muted); font-size:11px; }
		.graph-tabs button.active { color:var(--accent); border-color:var(--accent); }
		.stats { display:grid; grid-template-columns:repeat(3,1fr); border-top:1px solid var(--line); }
		.stat { padding:14px 16px; border-right:1px solid var(--line); }
		.stat:last-child { border:0; }
		.stat-label { display:block; color:var(--muted); font:10px ui-monospace, SFMono-Regular, Consolas, monospace; text-transform:uppercase; letter-spacing:.1em; }
		.stat-value { display:block; margin-top:5px; color:var(--text); font:20px ui-monospace, SFMono-Regular, Consolas, monospace; }
		.settings { padding:18px; }
		.field-grid { display:grid; grid-template-columns:1fr 1fr; gap:14px; }
		label { display:block; color:var(--muted); font:10px ui-monospace, SFMono-Regular, Consolas, monospace; text-transform:uppercase; letter-spacing:.1em; }
		label select, label input { display:block; width:100%; margin-top:7px; }
		input[type="range"] { accent-color:var(--accent); }
		.range-row { display:flex; align-items:center; gap:9px; }
		.range-row input { flex:1; }
		.range-value { min-width:42px; color:var(--accent); text-align:right; }
		.preset-bar { display:flex; align-items:center; gap:8px; padding:13px 18px; border-top:1px solid var(--line); }
		.preset-bar select { flex:1; }
		.share-bar { display:flex; gap:8px; padding:0 18px 15px; }
		.share-bar input { min-width:0; flex:1; color:var(--muted); border:1px solid var(--line); background:var(--panel-2); padding:9px 11px; font:11px ui-monospace, SFMono-Regular, Consolas, monospace; }
		.instrument-bar select { min-width:94px; padding:7px 8px; border-color:#4b554d; background:#202621; font:11px ui-monospace, SFMono-Regular, Consolas, monospace; }
		.instrument-bar button { min-width:34px; padding:7px 9px; border-color:#4b554d; }
		.header-unit { color:var(--muted); font:11px ui-monospace, SFMono-Regular, Consolas, monospace; }
		.header-status { overflow:hidden; color:var(--accent-2); font:11px ui-monospace, SFMono-Regular, Consolas, monospace; text-overflow:ellipsis; white-space:nowrap; }
		.hint { color:var(--muted); font:12px/1.5 Georgia, serif; }
		.status { color:var(--accent-2); }
		@media (max-width:850px) { .app { padding:20px 14px 30px; } header { align-items:flex-start; flex-direction:column; } .layout { grid-template-columns:1fr; } .editor-panel { min-height:520px; } }
		@media (max-width:500px) { .panel-head { align-items:flex-start; flex-direction:column; } .field-grid { grid-template-columns:1fr; } .stats { grid-template-columns:1fr; } .stat { border-right:0; border-bottom:1px solid var(--line); } }
	</style>
</head>
<body>
	<main class="app">
		<header>
			<div class="brand"><p class="kicker">atelier</p><h1>Bytebeat</h1></div>
			<div class="instrument-bar"><span class="readout"><b>t</b><span id="tValue">0</span></span><button class="primary" id="playBtn" title="Play">▶</button><button id="stopBtn" title="Stop">■</button><button id="rewindBtn" title="Reset sample counter">|◀</button><button id="loopBtn" title="Toggle loop">↻ Loop</button><button class="download" id="downloadBtn" title="Download WAV">⇩</button><select id="mode" title="Engine mode"><option value="bytebeat">Bytebeat</option><option value="floatbeat">Floatbeat</option><option value="funcbeat">Funcbeat</option></select><select id="sampleRate" title="Sample rate"><option value="4000">4000</option><option value="8000" selected>8000</option><option value="11025">11025</option><option value="16000">16000</option><option value="22050">22050</option><option value="44100">44100</option><option value="48000">48000</option></select><span class="header-unit">Hz</span><span class="header-status" id="status">ready</span></div>
		</header>
		<section class="layout">
			<article class="panel editor-panel">
				<div class="panel-head"><h2 class="panel-title">expression.js</h2><div class="controls"><button class="icon" id="randomBtn" title="Load a random formula">⤨</button></div></div>
				<div class="editor-wrap"><div class="line-no" id="lineNo">1</div><textarea id="code" spellcheck="false" aria-label="Bytebeat expression">t&lt;240000?t*(t%999)&amp;t&lt;&lt;3|t+t&amp;t&gt;&gt;9-t*999:t&lt;242800?0:((p=t-242800,n=(p/32000)|0,u=(p%32000)/(2**(n%6)),u*u-u-(u/2/u/u))&amp;u&lt;&lt;1&amp;u&gt;&gt;4)</textarea></div>
				<div class="editor-foot"><span>t = sample counter</span><span id="charCount">0 chars</span></div>
				<div class="preset-bar"><span class="panel-title">presets</span><select id="preset"><option value="classic">Classic pulse</option><option value="canyon">Canyon echo</option><option value="arcade">Arcade rain</option><option value="drone">Soft drone</option><option value="orbit">Orbiting tones</option></select><button id="loadPreset">Load</button></div>
				<div class="share-bar"><input id="shareUrl" placeholder="Paste a Dollchan #4 link or hash"><button id="importBtn">Import</button><button id="shareBtn">Share</button></div>
			</article>
			<div class="right-col">
				<article class="panel">
					<div class="panel-head"><h2 class="panel-title">signal / <span id="graphName">waveform</span></h2><span class="hint">live scope</span></div>
					<div class="scope-wrap"><div class="graph-tabs"><button class="active" data-graph="wave">wave</button><button data-graph="bars">bars</button><button data-graph="dots">dots</button><button data-graph="mirror">mirror</button></div><canvas class="scope" id="scope"></canvas></div>
					<div class="stats"><div class="stat"><span class="stat-label">signal</span><span class="stat-value" id="signalName">wave</span></div><div class="stat"><span class="stat-label">frequency</span><span class="stat-value" id="freqValue">0 Hz</span></div><div class="stat"><span class="stat-label">peak</span><span class="stat-value" id="peakValue">0.00</span></div></div>
				</article>
				<article class="panel settings"><div class="panel-head" style="margin:-18px -18px 18px"><h2 class="panel-title">machine settings</h2></div><div class="field-grid"><label>theme<select id="theme"><option value="atelier">Atelier</option><option value="cobalt">Cobalt signal</option><option value="sunset">Sunset circuit</option><option value="mono">Monochrome</option></select></label><label>volume<div class="range-row"><input id="volume" type="range" min="0" max="1" step=".01" value=".55"><span class="range-value" id="volumeValue">55%</span></div></label><label>tempo<div class="range-row"><input id="bpm" type="range" min="40" max="240" step="1" value="120"><span class="range-value" id="bpmValue">120</span></div></label><label>max beats<div class="range-row"><input id="maxBeats" type="range" min="1" max="10" step="1" value="10"><span class="range-value" id="maxBeatsValue">10</span></div></label></div><p class="hint" style="margin:17px 0 0">Downloads stop when the opening phrase repeats, with a hard limit of 10 beats.</p></article>
			</div>
		</section>
	</main>
	<script>
		const $ = id => document.getElementById(id);
		const code = $('code'), canvas = $('scope'), ctx = canvas.getContext('2d');
		const presets = {
			classic: '(t * (t >> 8 | t >> 9) & 46 & t >> 8) ^ (t * 5 >> 7 | t * 3 >> 10)',
			canyon: '((t >> 5) * (t >> 7) | (t >> 4)) + (t * 3 & t >> 10)',
			arcade: '(t * 9 & t >> 4 | t * 5 & t >> 7 | t * 3 & t >> 10) - 1',
			drone: 'Math.sin(t / 310) * 0.45 + Math.sin(t / 97) * 0.2',
			orbit: 'Math.sin(t / 30) * Math.sin(t / 700) * 0.8'
		};
		let audioContext, processor, gain, running = false, loopEnabled = false, loopSamples = 0, sampleT = 0, graph = 'wave', samples = new Float32Array(512), evaluator;
		const modeNames = ['bytebeat', 'bytebeat', 'floatbeat', 'funcbeat'];
		const base64ToBytes = value => Uint8Array.from(atob(value.replace(/-/g, '+').replace(/_/g, '/')), character => character.charCodeAt(0));
		const bytesToBase64 = bytes => btoa(String.fromCharCode(...bytes)).replace(/=+$/, '');
		const decodeDollchan = async hash => {
			const normalized = hash.trim().replace(/^.*#/, '#'); if (!normalized.startsWith('#4')) throw new Error('not a #4 link');
			const data = base64ToBytes(normalized.slice(2)); const rate = new DataView(data.buffer).getFloat32(1, true); const stream = new DecompressionStream('deflate-raw'); const writer = stream.writable.getWriter(); writer.write(data.slice(5)); writer.close(); const text = await new Response(stream.readable).text(); return { code: text, mode: modeNames[data[0]] || 'bytebeat', rate };
		};
		const encodeDollchan = async () => { const compressedStream = new CompressionStream('deflate-raw'); const writer = compressedStream.writable.getWriter(); writer.write(new TextEncoder().encode(code.value)); writer.close(); const compressed = new Uint8Array(await new Response(compressedStream.readable).arrayBuffer()); const data = new Uint8Array(5 + compressed.length); data[0] = { bytebeat: 0, floatbeat: 2, funcbeat: 3 }[$('mode').value]; new DataView(data.buffer).setFloat32(1, Number($('sampleRate').value), true); data.set(compressed, 5); return location.href.split('#')[0] + '#4' + bytesToBase64(data); };
		const updateLines = () => { $('lineNo').textContent = Array.from({length: Math.max(1, code.value.split('\n').length)}, (_, i) => i + 1).join('\n'); $('charCount').textContent = code.value.length + ' chars'; };
		const mathNames = 'const sin=Math.sin,cos=Math.cos,tan=Math.tan,asin=Math.asin,acos=Math.acos,atan=Math.atan,sqrt=Math.sqrt,abs=Math.abs,floor=Math.floor,ceil=Math.ceil,round=Math.round,min=Math.min,max=Math.max,pow=Math.pow,log=Math.log,exp=Math.exp,PI=Math.PI,random=Math.random,int=Math.floor;';
		const compile = () => { try { const mode = $('mode').value; evaluator = mode === 'funcbeat' ? new Function('t', mathNames + code.value) : new Function('t', mathNames + 'return (' + code.value + ')'); evaluator(0); $('status').textContent = 'compiled and ready'; return true; } catch (error) { $('status').textContent = 'syntax error'; return false; } };
		const valueAt = t => { let value = 0; try { value = Number(evaluator ? evaluator(t) : 0); } catch (error) { value = 0; } if (!Number.isFinite(value)) value = 0; if ($('mode').value === 'bytebeat') value = ((value & 255) - 128) / 128; return Math.max(-1, Math.min(1, value)); };
		const makeWav = (samplesToWrite, sampleRate) => { const buffer = new ArrayBuffer(44 + samplesToWrite.length * 2), view = new DataView(buffer); const write = (offset, text) => [...text].forEach((character, index) => view.setUint8(offset + index, character.charCodeAt(0))); write(0, 'RIFF'); view.setUint32(4, 36 + samplesToWrite.length * 2, true); write(8, 'WAVE'); write(12, 'fmt '); view.setUint32(16, 16, true); view.setUint16(20, 1, true); view.setUint16(22, 1, true); view.setUint32(24, sampleRate, true); view.setUint32(28, sampleRate * 2, true); view.setUint16(32, 2, true); view.setUint16(34, 16, true); write(36, 'data'); view.setUint32(40, samplesToWrite.length * 2, true); samplesToWrite.forEach((sample, index) => view.setInt16(44 + index * 2, Math.max(-1, Math.min(1, sample)) * 32767, true)); return new Blob([view], { type: 'audio/wav' }); };
		const downloadWav = () => { if (!compile()) return; const sampleRate = Number($('sampleRate').value), maxBeats = Math.min(10, Number($('maxBeats').value)), beatSamples = Math.round(sampleRate * 60 / Number($('bpm').value)), limit = beatSamples * maxBeats, opening = new Float32Array(256), output = []; for (let i = 0; i < limit; i++) { const sample = valueAt(i); if (i < opening.length) opening[i] = sample; output.push(sample); if (i >= beatSamples && i % beatSamples === 0) { let error = 0; for (let j = 0; j < opening.length; j++) error += Math.abs(opening[j] - output[i - beatSamples + j]); if (error / opening.length < .008) break; } } const link = document.createElement('a'); link.href = URL.createObjectURL(makeWav(output, sampleRate)); link.download = 'bytebeat-' + output.length + 'samples.wav'; link.click(); setTimeout(() => URL.revokeObjectURL(link.href), 1000); $('status').textContent = 'downloaded ' + (output.length / sampleRate).toFixed(2) + 's WAV'; };
		const draw = () => { const dpr = window.devicePixelRatio || 1, rect = canvas.getBoundingClientRect(); if (canvas.width !== rect.width * dpr || canvas.height !== rect.height * dpr) { canvas.width = rect.width * dpr; canvas.height = rect.height * dpr; } const w = canvas.width, h = canvas.height; ctx.clearRect(0, 0, w, h); ctx.fillStyle = '#0e100f'; ctx.fillRect(0, 0, w, h); ctx.strokeStyle = 'rgba(255,255,255,.08)'; ctx.lineWidth = dpr; for (let x = 0; x < w; x += w / 8) { ctx.beginPath(); ctx.moveTo(x, 0); ctx.lineTo(x, h); ctx.stroke(); } ctx.beginPath(); ctx.moveTo(0, h / 2); ctx.lineTo(w, h / 2); ctx.stroke(); ctx.strokeStyle = getComputedStyle(document.documentElement).getPropertyValue('--accent').trim(); ctx.fillStyle = ctx.strokeStyle; ctx.lineWidth = 2 * dpr;
			if (graph === 'bars') { const bw = w / samples.length; samples.forEach((v, i) => { const bh = Math.abs(v) * h * .45; ctx.fillRect(i * bw, v >= 0 ? h / 2 - bh : h / 2, Math.max(1, bw - 1), bh); }); }
			else if (graph === 'dots') { samples.forEach((v, i) => ctx.fillRect(i / samples.length * w, h / 2 - v * h * .43, 3 * dpr, 3 * dpr)); }
			else { ctx.beginPath(); samples.forEach((v, i) => { const x = i / (samples.length - 1) * w, y = h / 2 - v * h * .42; i ? ctx.lineTo(x, y) : ctx.moveTo(x, y); }); ctx.stroke(); if (graph === 'mirror') { ctx.globalAlpha = .25; ctx.beginPath(); samples.forEach((v, i) => { const x = i / (samples.length - 1) * w, y = h / 2 + v * h * .42; i ? ctx.lineTo(x, y) : ctx.moveTo(x, y); }); ctx.stroke(); ctx.globalAlpha = 1; } }
			$('tValue').textContent = Math.floor(sampleT).toLocaleString(); const peak = Math.max(...samples.map(Math.abs)); $('peakValue').textContent = peak.toFixed(2); $('freqValue').textContent = Math.round(($('sampleRate').value / 256) * Math.abs(samples[0] || 0) + 110) + ' Hz'; requestAnimationFrame(draw);
		};
		const start = () => { if (running) return; if (!compile()) return; audioContext = new (window.AudioContext || window.webkitAudioContext)(); const rate = Number($('sampleRate').value); loopSamples = Math.round(rate * 60 / Number($('bpm').value)) * Math.min(10, Number($('maxBeats').value)); processor = audioContext.createScriptProcessor(1024, 0, 1); gain = audioContext.createGain(); gain.gain.value = Number($('volume').value); processor.onaudioprocess = event => { const output = event.outputBuffer.getChannelData(0); for (let i = 0; i < output.length; i++) { if (loopEnabled && loopSamples) sampleT %= loopSamples; const v = valueAt(sampleT++); output[i] = v; samples[i % samples.length] = valueAt(loopEnabled && loopSamples ? (sampleT + i * 3) % loopSamples : sampleT + i * 3); } }; processor.connect(gain); gain.connect(audioContext.destination); running = true; $('playBtn').textContent = '● Playing'; $('status').textContent = (loopEnabled ? 'looping ' : 'running ') + 'at ' + (rate / 1000) + 'kHz'; };
		const stop = () => { if (processor) processor.disconnect(); if (gain) gain.disconnect(); if (audioContext) audioContext.close(); processor = null; running = false; $('playBtn').textContent = '▶ Play'; $('status').textContent = 'stopped'; };
		document.addEventListener('visibilitychange', () => { if (document.hidden && running) stop(); });
		code.addEventListener('input', () => { updateLines(); if (running) compile(); }); code.addEventListener('keydown', event => { if (event.key === 'Tab') { event.preventDefault(); const start = code.selectionStart; code.value = code.value.slice(0, start) + '  ' + code.value.slice(code.selectionEnd); code.selectionStart = code.selectionEnd = start + 2; updateLines(); } });
		$('playBtn').onclick = start; $('stopBtn').onclick = stop; $('downloadBtn').onclick = downloadWav; $('loopBtn').onclick = () => { loopEnabled = !loopEnabled; $('loopBtn').textContent = loopEnabled ? '↻ Loop: on' : '↻ Loop'; $('loopBtn').classList.toggle('primary', loopEnabled); if (running) $('status').textContent = loopEnabled ? 'loop enabled' : 'loop disabled'; }; $('loadPreset').onclick = () => { code.value = presets[$('preset').value]; updateLines(); compile(); }; $('randomBtn').onclick = () => { const keys = Object.keys(presets); $('preset').value = keys[Math.floor(Math.random() * keys.length)]; $('loadPreset').click(); };
		$('importBtn').onclick = async () => { try { const imported = await decodeDollchan($('shareUrl').value); code.value = imported.code; $('mode').value = imported.mode; $('sampleRate').value = imported.rate; updateLines(); compile(); $('status').textContent = 'Dollchan link imported'; } catch (error) { $('status').textContent = 'could not read that link'; } };
		$('shareBtn').onclick = async () => { try { const url = await encodeDollchan(); $('shareUrl').value = url; await navigator.clipboard.writeText(url); $('status').textContent = 'share link copied'; } catch (error) { $('status').textContent = 'sharing needs a secure browser context'; } };
		$('theme').onchange = event => document.documentElement.dataset.theme = event.target.value === 'atelier' ? '' : event.target.value; $('volume').oninput = event => { $('volumeValue').textContent = Math.round(event.target.value * 100) + '%'; if (gain) gain.gain.value = event.target.value; }; $('bpm').oninput = event => { $('bpmValue').textContent = event.target.value; if (running) loopSamples = Math.round(Number($('sampleRate').value) * 60 / Number(event.target.value)) * Math.min(10, Number($('maxBeats').value)); }; $('maxBeats').oninput = event => { $('maxBeatsValue').textContent = event.target.value; if (running) loopSamples = Math.round(Number($('sampleRate').value) * 60 / Number($('bpm').value)) * Math.min(10, Number(event.target.value)); }; $('mode').onchange = compile; $('rewindBtn').onclick = () => { sampleT = 0; samples.fill(0); $('status').textContent = 'counter reset'; };
		document.querySelectorAll('[data-graph]').forEach(button => button.onclick = () => { document.querySelectorAll('[data-graph]').forEach(item => item.classList.remove('active')); button.classList.add('active'); graph = button.dataset.graph; $('graphName').textContent = button.textContent; });
		updateLines(); compile(); for (let i = 0; i < samples.length; i++) samples[i] = valueAt(i); draw(); if (location.hash.startsWith('#4')) { decodeDollchan(location.hash).then(imported => { code.value = imported.code; $('mode').value = imported.mode; $('sampleRate').value = imported.rate; updateLines(); compile(); $('status').textContent = 'Dollchan link imported'; }).catch(() => { $('status').textContent = 'could not read that link'; }); }
	</script>
</body>
</html>
