<script lang="ts">
	let conversation = [
		{ speaker: "Paciente", text: "Tengo dolor de cabeza desde hace 3 días." },
		{ speaker: "Doctor", text: "¿El dolor es constante o intermitente?" },
		{ speaker: "Paciente", text: "Es constante y empeora por la tarde." },
		{ speaker: "Doctor", text: "Entiendo. Vamos a hacerte unos exámenes." }
	];

	let currentLine = 0;
	let transcribedText = "";

	function simulateTranscription() {
		if (currentLine < conversation.length) {
			const line = conversation[currentLine];
			transcribedText += `${line.speaker}: ${line.text}\n`;
			currentLine += 1;
		}
	}

	function startTranscription() {
		currentLine = 0;
		transcribedText = "";

		const interval = setInterval(() => {
			simulateTranscription();
			if (currentLine === conversation.length) {
				clearInterval(interval); // Stop when all lines are transcribed
			}
		}, 2000); // Change every 3 seconds
	}

	// Restart the transcription every 20 seconds
	setInterval(startTranscription, 20000);

	// Start the first transcription immediately
	startTranscription();
</script>



<section class="relative overflow-hidden ">
	<div class="container mx-auto px-4 py-12 lg:py-24 flex flex-col items-center text-center">
		<!-- Centered Text Content -->
		<h1 class="text-3xl font-bold tracking-tight text-gray-900 sm:text-4xl lg:text-5xl">
			Speech Solutions: Phonetic Transcription
		</h1>
		<p class="mt-4 text-lg text-gray-600 max-w-2xl">
			Nuestra plataforma SaaS utiliza IA avanzada para transcribir conversaciones médicas y empresariales, devolviéndolas en formato conversacional preciso y natural.
		</p>
		<div class="mt-6">
			<a href="/#" class="btn btn-primary btn-wide rounded-md text-semibold text-lg">Empieza Gratis</a>
		</div>
	</div>
</section>

<div class="bg-gray-50 py-12 lg:py-16">
	<div class="container mx-auto px-8 lg:px-14 flex flex-col lg:flex-row items-start justify-between gap-8">
		<!-- Left side: Text and product information -->
		<div class="lg:w-1/2">
      <span class="inline-block bg-green-100 text-green-800 px-2 py-1 rounded-full text-xs font-semibold">
        IA Conversacional
      </span>
			<h2 class="mt-4 text-3xl font-bold text-gray-900">Transcripción Inteligente</h2>
			<p class="mt-2 text-lg text-gray-600">Innovación para cada conversación</p>
			<hr class="mt-4 mb-6 border-gray-200">
			<p class="text-base text-gray-600">
				Esta es una herramienta de IA conversacional que permite transcripciones de voz naturales, receptivas y seguras. Construido con tecnología de vanguardia en reconocimiento automático de voz, esta solución asegura que cada conversación sea fluida y precisa, independientemente del acento, idioma o entorno.
			</p>
			<div class="mt-6">
				<a href="/#" class="btn btn-primary btn-sm">Pruebalo ahora</a>
			</div>
		</div>

		<!-- Right side: Secondary image or content -->
		<div class="lg:w-1/2">
			<div class="transcription-box">
				<h3 class="text-lg font-bold mb-2">Transcripción en tiempo real</h3>
				<pre>{transcribedText}<span class="cursor"></span></pre>
			</div>
		</div>
</div>
</div>

<style>
    .transcription-box {
        background-color: #f9fafb;
        border: 1px solid #e5e7eb;
        padding: 16px;
        border-radius: 8px;
        white-space: pre-wrap;
        font-family: "Courier New", Courier, monospace;
        min-height: 150px;
        box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
    }

    .cursor {
        display: inline-block;
        width: 2px;
        height: 1em;
        background-color: black;
        animation: blink 1s steps(1) infinite;
        vertical-align: bottom;
    }

    @keyframes blink {
        50% {
            opacity: 0;
        }
    }
</style>



