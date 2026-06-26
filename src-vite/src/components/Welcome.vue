<template>
  <div class="absolute inset-0 flex items-center justify-center px-6" data-tauri-drag-region>
    <div class="relative max-w-2xl w-full text-center">
      <div class="relative mb-7 flex flex-col items-center gap-4">
        <div class="rounded-box border border-primary/15 bg-base-100/70 p-3 shadow-sm">
          <img :src="iconLogo" class="w-16 h-16 select-none [-webkit-app-region:no-drag]" draggable="false" />
        </div>
        <div>
          <h2 class="text-2xl font-black text-base-content">
            {{ $t('welcome.title') }}
          </h2>
          <p class="mt-3 text-sm leading-6 text-base-content/60">
            {{ $t('welcome.description') }}
          </p>
        </div>
      </div>

      <div class="relative rounded-box border border-primary/15 bg-base-100/70 p-5 shadow-[0_18px_60px_rgba(242,111,159,0.16)] backdrop-blur">
        <div class="mb-4 flex items-center justify-center gap-2 text-sm font-bold text-primary">
          <IconFolder class="w-5 h-5" />
          <span>{{ $t('welcome.nikki_path_label') }}</span>
        </div>
        <div class="mx-auto mb-5 max-w-full overflow-hidden text-ellipsis whitespace-nowrap rounded-box bg-base-200/70 px-3 py-2 text-xs font-semibold text-base-content/55">
          {{ NIKKI_SCREENSHOT_DIR }}
        </div>
        <div class="flex flex-col justify-center gap-3 sm:flex-row">
          <button class="btn btn-primary min-w-40 rounded-box" @click="requestSetupNikkiAlbum">
            <IconAdd class="w-4 h-4" />
            {{ $t('welcome.open_nikki_album') }}
          </button>
          <button class="btn min-w-40 rounded-box border-primary/20 bg-base-100/80 text-base-content/70" @click="requestAddAlbum">
            <IconFolder class="w-4 h-4" />
            {{ $t('welcome.choose_folder') }}
          </button>
        </div>
      </div>

      <div class="mt-4 grid grid-cols-1 gap-2 text-left sm:grid-cols-3">
        <div class="rounded-box border border-base-content/5 bg-base-100/50 px-3 py-2 text-xs font-semibold text-base-content/55">
          {{ $t('welcome.quick_note_1') }}
        </div>
        <div class="rounded-box border border-base-content/5 bg-base-100/50 px-3 py-2 text-xs font-semibold text-base-content/55">
          {{ $t('welcome.quick_note_2') }}
        </div>
        <div class="rounded-box border border-base-content/5 bg-base-100/50 px-3 py-2 text-xs font-semibold text-base-content/55">
          {{ $t('welcome.quick_note_3') }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { emit as tauriEmit } from '@tauri-apps/api/event';
import { IconAdd, IconFolder } from '@/common/icons';
import { NIKKI_SCREENSHOT_DIR } from '@/common/nikki';
import iconLogo from '@/assets/images/icon.png';

const requestAddAlbum = () => {
  tauriEmit('add-album-requested');
};

const requestSetupNikkiAlbum = () => {
  tauriEmit('setup-nikki-album-requested');
};
</script>
