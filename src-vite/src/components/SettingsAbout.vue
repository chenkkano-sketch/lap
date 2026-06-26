<template>
  <div class="flex flex-col items-start justify-start gap-4 h-full text-base-content/70 cursor-default">

    <!-- logo -->
    <div class="px-2 flex w-full flex-row items-center justify-start gap-4">
      <div class="shrink-0">
        <img :src="iconLogo" class="w-20 h-20 select-none [-webkit-app-region:no-drag]" draggable="false" />
      </div>
      <div class="flex flex-col text-left">
        <h3 class="text-xl">{{ NIKKI_APP_NAME }}</h3>
        <p class="mt-2">{{ $t('settings.about.package.app_description') }}</p>
      </div>
    </div>

    <!-- author notice -->
    <div class="w-full max-w-lg rounded-box border border-primary/15 bg-primary/5 p-4 text-left shadow-sm">
      <div class="mb-3 flex items-center justify-between gap-3">
        <h4 class="text-sm font-semibold text-base-content">
          {{ $t('settings.about.author_notice.title') }}
        </h4>
        <span class="badge badge-primary badge-sm">{{ NIKKI_AUTHOR_NAME }}</span>
      </div>

      <p class="text-sm leading-6 text-base-content/70">
        {{ NIKKI_FORK_NOTE }}
      </p>

      <div class="mt-4 grid gap-2 text-sm">
        <a
          v-for="link in authorLinks"
          :key="link.href"
          :href="link.href"
          target="_blank"
          class="flex items-center justify-between gap-3 rounded-box bg-base-100/70 px-3 py-2 transition-colors hover:bg-base-100 hover:text-primary"
        >
          <span class="flex min-w-0 items-center gap-2">
            <component :is="link.icon" class="t-icon-size-sm shrink-0" />
            <span class="truncate">{{ link.label }}</span>
          </span>
          <IconExternal class="t-icon-size-sm shrink-0 opacity-50" />
        </a>
      </div>
    </div>

    <!-- package info -->
    <div class="w-full max-w-lg rounded-box border border-base-content/5 bg-base-300/30 p-4 shadow-sm">
      <div class="space-y-3 text-left">
        <div class="grid grid-cols-[84px_1fr] items-start gap-3 text-sm">
          <div class="text-base-content/30">
            {{ $t('settings.about.package.version') }}
          </div>
          <div class="flex items-center gap-2">
            <span>{{ displayVersion }}</span>
            <button
              class="badge badge-sm border-0 px-2 py-2 font-medium transition-colors hover:text-primary"
              :class="isUpdateActionEnabled ? 'badge-primary cursor-pointer' : 'badge-neutral/60 cursor-pointer'"
              :disabled="isInstallingUpdate || isCheckingUpdate"
              :title="updateButtonTooltip"
              @click="handleUpdateAction"
            >
              <span v-if="isInstallingUpdate || isCheckingUpdate" class="loading loading-spinner loading-xs"></span>
              <span>{{ updateButtonText }}</span>
            </button>
          </div>
        </div>

        <div class="grid grid-cols-[84px_1fr] items-start gap-3 text-sm">
          <div class="text-base-content/30">
            {{ $t('settings.about.package.build_time') }}
          </div>
          <div>{{ buildTime }}</div>
        </div>

        <div class="grid grid-cols-[84px_1fr] items-start gap-3 text-sm">
          <div class="text-base-content/30">
            {{ $t('settings.about.package.license') }}
          </div>
          <div>{{ packageInfo.license }}</div>
        </div>

        <div class="grid grid-cols-[84px_1fr] items-center gap-1 text-sm">
          <div class="text-base-content/30">
            {{ $t('settings.about.package.link') }}
          </div>
          <div class="flex flex-wrap items-center justify-start">
            <a
              :href="packageInfo.repository"
              target="_blank"
              class="inline-flex items-center gap-1.5 rounded-box px-2 py-1 text-xs transition-colors hover:bg-base-100/50 hover:text-primary"
            >
              <IconGithub class="t-icon-size-sm" />
              <span>{{ $t('settings.about.package.github') }}</span>
            </a>
            <a
              :href="issuesUrl"
              target="_blank"
              class="inline-flex items-center gap-1.5 rounded-box px-2 py-1 text-xs transition-colors hover:bg-base-100/50 hover:text-primary"
            >
              <IconFocus class="t-icon-size-sm" />
              <span>{{ $t('settings.about.package.feedback') }}</span>
            </a>
            <a
              :href="privacyUrl"
              target="_blank"
              class="inline-flex items-center gap-1.5 rounded-box px-2 py-1 text-xs transition-colors hover:bg-base-100/50 hover:text-primary"
            >
              <IconLock class="t-icon-size-sm" />
              <span>{{ $t('settings.about.package.privacy') }}</span>
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref, onMounted } from 'vue';
import { useI18n } from 'vue-i18n';
import { getPackageInfo, getBuildTime } from '@/common/api';
import { useAppUpdater } from '@/common/updater';
import { IconExternal, IconFocus, IconGithub, IconLink, IconLock } from '@/common/icons';
import {
  NIKKI_APP_NAME,
  NIKKI_AUTHOR_NAME,
  NIKKI_BLOG_URL,
  NIKKI_FORK_NOTE,
  NIKKI_NEW_PROJECT_URL,
  NIKKI_OLD_PROJECT_URL,
  NIKKI_TAVERN_URL,
} from '@/common/nikki';
import iconLogo from '@/assets/images/icon.png';

const packageInfo = ref<any>({
  name: '',
  description: '',
  version: '',
  commit_hash: '',
  license: '',
  authors: [],
  homepage: '',
  repository: ''
});
const buildTime = ref('');
const displayVersion = computed(() => {
  const version = packageInfo.value.version || '';
  const commitHash = packageInfo.value.commit_hash || packageInfo.value.commitHash || '';
  return commitHash ? `${version} (${commitHash})` : version;
});
const privacyUrl = computed(() => {
  const repo = packageInfo.value.repository || '';
  if (!repo) return 'https://github.com/chenkkano-sketch/lap/blob/main/PRIVACY.md';
  return repo.endsWith('/') ? `${repo}blob/main/PRIVACY.md` : `${repo}/blob/main/PRIVACY.md`;
});
const issuesUrl = computed(() => {
  const repo = packageInfo.value.repository || '';
  if (!repo) return 'https://github.com/chenkkano-sketch/lap/issues';
  return repo.endsWith('/') ? `${repo}issues` : `${repo}/issues`;
});
const { locale, messages } = useI18n();
const localeMsg = computed(() => messages.value[locale.value] as any);
const authorLinks = computed(() => [
  {
    label: localeMsg.value?.settings?.about?.author_notice?.blog || '博客',
    href: NIKKI_BLOG_URL,
    icon: IconLink,
  },
  {
    label: localeMsg.value?.settings?.about?.author_notice?.tavern || '博客留言板',
    href: NIKKI_TAVERN_URL,
    icon: IconFocus,
  },
  {
    label: localeMsg.value?.settings?.about?.author_notice?.new_project || '新的项目地址',
    href: NIKKI_NEW_PROJECT_URL,
    icon: IconGithub,
  },
  {
    label: localeMsg.value?.settings?.about?.author_notice?.old_project || '旧项目地址',
    href: NIKKI_OLD_PROJECT_URL,
    icon: IconGithub,
  },
]);
const {
  isCheckingUpdate,
  isInstallingUpdate,
  updateButtonTooltip,
  updateButtonText,
  isUpdateActionEnabled,
  handleUpdateAction,
} = useAppUpdater(localeMsg, { toastPlacement: 'center' });

onMounted(async () => {
  try {
    packageInfo.value = await getPackageInfo();
    const time = await getBuildTime();
    buildTime.value = time || '';
  } catch (error) {
    console.error('Failed to load about info:', error);
  }
});
</script>
