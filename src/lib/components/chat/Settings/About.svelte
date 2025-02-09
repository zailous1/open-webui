<script lang="ts">
	import { getVersionUpdates } from '$lib/apis';
	import { getOllamaVersion } from '$lib/apis/ollama';
	import { WEBUI_BUILD_HASH, WEBUI_VERSION } from '$lib/constants';
	import { WEBUI_NAME, config, showChangelog } from '$lib/stores';
	import { compareVersion } from '$lib/utils';
	import { onMount, getContext } from 'svelte';

	import Tooltip from '$lib/components/common/Tooltip.svelte';

	const i18n = getContext('i18n');

	let ollamaVersion = '';

	let updateAvailable = null;
	let version = {
		current: '',
		latest: ''
	};

	const checkForVersionUpdates = async () => {
		updateAvailable = null;
		version = await getVersionUpdates(localStorage.token).catch((error) => {
			return {
				current: WEBUI_VERSION,
				latest: WEBUI_VERSION
			};
		});

		console.log(version);

		updateAvailable = compareVersion(version.latest, version.current);
		console.log(updateAvailable);
	};

	onMount(async () => {
		ollamaVersion = await getOllamaVersion(localStorage.token).catch((error) => {
			return '';
		});

		checkForVersionUpdates();
	});
</script>

<div class="flex flex-col h-full justify-between space-y-3 text-sm mb-6">
  <div class="space-y-3 overflow-y-scroll max-h-[28rem] lg:max-h-full">

    <!-- About Care Advisor -->
    <div>
      <div class="mb-2.5 text-sm font-medium">About Care Advisor</div>
      <div class="text-xs text-gray-700 dark:text-gray-200">
        Care Advisor is a proprietary AI-powered application developed by Zailous LLC to assist caregivers in understanding and supporting individuals under their care. This application has been built upon open-source technologies, incorporating innovative enhancements to deliver a tailored caregiving experience.
      </div>
    </div>

    <hr class="dark:border-gray-850" />

    <!-- Licensing & Acknowledgements -->
    <div>
      <div class="mb-2.5 text-sm font-medium">Licensing &amp; Acknowledgements</div>
      <div class="text-xs text-gray-700 dark:text-gray-200">
        Care Advisor includes components derived from open-source software and adheres to the respective licenses governing their use.
      </div>
    </div>

    <hr class="dark:border-gray-850" />

    <!-- Attributions -->
    <div>
      <div class="mb-2.5 text-sm font-medium">Attributions</div>
      <div class="text-xs text-gray-400 dark:text-gray-500">
        Emoji graphics provided by Twemoji, licensed under CC-BY 4.0.
      </div>
    </div>

    <hr class="dark:border-gray-850" />

    <!-- Open WebUI (Base Framework & Components) -->
    <div>
      <div class="mb-2.5 text-sm font-medium">Open WebUI (Base Framework &amp; Components)</div>
      <div class="text-xs text-gray-700 dark:text-gray-200">
        Portions of this software are derived from Open WebUI, developed by Timothy Jaeryang Baek, and are used in accordance with the following license:
      </div>
      <div>
        <pre class="mt-2 text-xs text-gray-400 dark:text-gray-500">
Copyright (c) 2025 Open WebUI (Timothy Jaeryang Baek)
All rights reserved.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

Redistributions of source code must retain the above copyright notice, this list of conditions, and the following disclaimer.
Redistributions in binary form must reproduce the above copyright notice, this list of conditions, and the following disclaimer in the documentation and/or other materials provided with the distribution.
Neither the name of the copyright holder nor the names of its contributors may be used to endorse or promote products derived from this software without specific prior written permission.
THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
        </pre>
      </div>
    </div>

    <hr class="dark:border-gray-850" />

    <!-- Created by -->
    <div class="mt-2 text-xs text-gray-400 dark:text-gray-500">
      Created by Timothy J. Baek
    </div>

  </div>
</div>

