<script>
	import {faChartLine, faClock, faCog, faUsers} from '@fortawesome/free-solid-svg-icons';
	import Fa from 'svelte-fa';

	// Navigation links
	const navigationLinks = [
		{ name: 'Características', href: '#', active: true },
		{ name: 'Cómo funciona', href: '/how-it-works' },
		{ name: 'Casos de Uso', href: '/eu-isolation' },
		{ name: 'Nombre', href: '/ga-importer' },
	];
</script>

<section class="px-10 py-16 bg-white">
	<div class="container mx-auto max-w-7xl flex">
		<!-- Sidebar Navigation -->
		<aside class="hidden lg:block w-1/4 pr-8">
			<div class="sticky top-24 space-y-8">
				<!-- Learn More Section -->
				<div>
					<h3 class="text-sm font-semibold text-gray-500 uppercase mb-4">Aprende más sobre</h3>
					<ul class="space-y-3">
						{#each navigationLinks as link}
							<li>
								<a href={link.href} class={`text-md font-medium ${link.active ? 'text-indigo-600' : 'text-gray-700 hover:text-indigo-500'}`}>
									{link.name}
								</a>
							</li>
						{/each}
					</ul>
				</div>
			</div>
		</aside>

		<!-- Main Content -->
		<div class="w-full lg:w-3/4">
			<!-- Main Heading and Introduction -->
			<h1 class="text-6xl font-extrabold text-gray-900 mb-6 leading-tight">
				Características de nuestro software de transcripción
			</h1>
			<p class="text-lg text-gray-600 mb-8">
				Nuestro software de transcripción es fácil de configurar y funciona perfectamente con cualquier flujo de trabajo. Explora todas las características (ver abajo) sin ninguna complejidad.
			</p>
			<p class="text-lg text-gray-600 mb-12">
				Nuestro software está diseñado para hacer el análisis de audio sin esfuerzo, confiado por profesionales que van desde educadores hasta líderes empresariales.
			</p>

			<!-- Feature Blocks -->
			<div class="grid grid-cols-1 lg:grid-cols-2 gap-12 mb-16">
				<!-- Feature Block 1 -->
				<div class="flex items-start">
					<div class="mr-4">
						<Fa icon={faChartLine} class="h-10 w-10 text-indigo-600" />
					</div>
					<div>
						<h2 class="text-3xl font-semibold text-gray-800 mb-2">Transcripciones Precisas</h2>
						<p class="text-gray-600">
							Obtén transcripciones altamente precisas con nuestra tecnología avanzada de IA. Ideal para todo tipo de contenido de audio, desde entrevistas hasta reuniones.
						</p>
					</div>
				</div>

				<!-- Feature Block 2 -->
				<div class="flex items-start">
					<div class="mr-4">
						<Fa icon={faClock} class="h-10 w-10 text-indigo-600" />
					</div>
					<div>
						<h2 class="text-3xl font-semibold text-gray-800 mb-2">Procesamiento en Tiempo Real</h2>
						<p class="text-gray-600">
							Obtén transcripciones y ediciones al instante. A diferencia de otras herramientas, nuestro software procesa audio en tiempo real, permitiéndote revisar y hacer ajustes inmediatamente.
						</p>
					</div>
				</div>

				<!-- Feature Block 3 -->
				<div class="flex items-start">
					<div class="mr-4">
						<Fa icon={faCog} class="h-10 w-10 text-indigo-600" />
					</div>
					<div>
						<h2 class="text-3xl font-semibold text-gray-800 mb-2">Interfaz Personalizable</h2>
						<p class="text-gray-600">
							Adapta la interfaz a tu flujo de trabajo. Nuestro software ofrece herramientas flexibles que se adaptan a diversas necesidades profesionales, desde el periodismo hasta la documentación legal.
						</p>
					</div>
				</div>

				<!-- Feature Block 4 -->
				<div class="flex items-start">
					<div class="mr-4">
						<Fa icon={faUsers} class="h-10 w-10 text-indigo-600" />
					</div>
					<div>
						<h2 class="text-3xl font-semibold text-gray-800 mb-2">Herramientas de Colaboración</h2>
						<p class="text-gray-600">
							Colabora en tiempo real con tu equipo. Nuestro software de transcripción facilita compartir notas, resúmenes y archivos de audio para un trabajo en equipo sin fisuras.
						</p>
					</div>
				</div>
			</div>

			<!-- CTA Button -->
			<div class="text-center">
				<a href="/free-trial" class="btn btn-primary py-4 px-8 rounded-md font-semibold text-white shadow-lg hover:bg-indigo-600 transition-colors duration-200">
					Empieza una prueba gratuita
				</a>
				<p class="mt-6 text-gray-500 text-sm">Configuración rápida • Gratis por 30 días • Sin obligación</p>
			</div>
		</div>
	</div>
</section>
