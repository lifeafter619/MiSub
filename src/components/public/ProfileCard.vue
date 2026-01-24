<script setup>
import QRCodeOverlay from './QRCodeOverlay.vue';
import BaseIcon from '../ui/BaseIcon.vue';

const props = defineProps({
  profile: {
    type: Object,
    required: true
  },
  isQrExpanded: {
    type: Boolean,
    default: false
  }
});

const emit = defineEmits([
  'quick-import',
  'toggle-qr',
  'preview',
  'copy-link',
  'download-qr',
  'register-canvas'
]);

// Icon Paths
const ICONS = {
  import: 'M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4',
  qr: 'M12 4v1m6 11h2m-6 0h-2v4m0-11v3m0 0h.01M12 12h4.01M16 20h4M4 12h4m12 0h.01M5 8h2a1 1 0 001-1V5a1 1 0 00-1-1H5a1 1 0 00-1 1v2a1 1 0 001 1zm12 0h2a1 1 0 001-1V5a1 1 0 00-1-1h-2a1 1 0 00-1 1v2a1 1 0 001 1zM5 20h2a1 1 0 001-1v-2a1 1 0 00-1-1H5a1 1 0 00-1 1v2a1 1 0 001 1z',
  preview: 'M15 12a3 3 0 11-6 0 3 3 0 016 0z M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z',
  link: 'M13.828 10.172a4 4 0 00-5.656 0l-4 4a4 4 0 105.656 5.656l1.102-1.101m-.758-4.899a4 4 0 005.656 0l4-4a4 4 0 00-5.656-5.656l-1.1 1.1'
};
</script>

<template>
  <div
    class="group relative h-full flex flex-col bg-white dark:bg-[#1a1d29] border border-gray-100 dark:border-white/5 rounded-3xl p-6 transition-all duration-300 hover:shadow-xl hover:shadow-primary-500/5 hover:border-primary-500/30 hover:-translate-y-1"
  >
    <!-- Top Right Buttons: Quick Import & QR Code -->
    <div class="absolute top-5 right-5 flex gap-2">
      <!-- Quick Import Button -->
      <button
        type="button"
        @click="emit('quick-import', profile)"
        class="w-10 h-10 flex items-center justify-center rounded-xl bg-primary-50 dark:bg-primary-500/10 border border-primary-100 dark:border-primary-500/20 text-primary-600 dark:text-primary-400 hover:bg-primary-100 dark:hover:bg-primary-500/30 transition-all hover:scale-110 group/import focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-primary-500/50"
        title="一键导入"
        aria-label="一键导入"
      >
        <BaseIcon :path="ICONS.import" className="w-5 h-5" />
        <span
          class="absolute -bottom-8 right-0 px-2 py-1 bg-gray-900 dark:bg-gray-700 text-white text-xs rounded opacity-0 group-hover/import:opacity-100 transition-opacity whitespace-nowrap pointer-events-none"
        >
          <span class="text-2xl drop-shadow-sm">🚀</span>
        </div>
        <h3
          class="text-lg font-bold text-gray-900 dark:text-white line-clamp-1"
          :title="profile.name"
        >
          {{ profile.name }}
        </h3>
      </div>
      
      <!-- QR Toggle (Small Top Right) -->
      <button
        type="button"
        @click="emit('toggle-qr', profile)"
        class="w-10 h-10 flex items-center justify-center rounded-xl bg-teal-50 dark:bg-teal-500/10 border border-teal-100 dark:border-teal-500/20 text-teal-600 dark:text-teal-400 hover:bg-teal-100 dark:hover:bg-teal-500/30 transition-all hover:scale-110 group/qr focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-teal-500/50"
        :class="{ 'bg-teal-100 dark:bg-teal-500/30': isQrExpanded }"
        title="查看二维码"
        :aria-label="isQrExpanded ? '收起二维码' : '查看二维码'"
      >
        <BaseIcon :path="ICONS.qr" className="w-5 h-5" />
      </button>
    </div>

    <!-- Body: Description -->
    <div class="flex-1 mb-6">
       <p class="text-gray-500 dark:text-gray-400 text-sm leading-relaxed line-clamp-3">
        {{ profile.description || '暂无简介' }}
      </p>
    </div>

    <!-- Footer: Action Bar -->
    <div class="mt-auto pt-4 border-t border-gray-100 dark:border-white/5 flex items-center gap-3">
        <!-- Primary Action: Import -->
        <button
          @click="emit('quick-import', profile)"
          class="flex-1 flex items-center justify-center gap-2 px-4 py-2.5 bg-primary-600 hover:bg-primary-700 text-white text-sm font-bold rounded-xl shadow-lg shadow-primary-600/20 transition-all active:scale-95"
        >
          <BaseIcon :path="ICONS.import" className="w-4 h-4" />
          一键导入
        </button>

        <!-- Secondary Actions -->
        <button
          @click="emit('preview', profile)"
          class="p-2.5 rounded-xl border border-gray-200 dark:border-gray-700 text-gray-500 dark:text-gray-400 hover:bg-gray-50 dark:hover:bg-gray-800 hover:text-primary-600 dark:hover:text-primary-400 transition-colors"
          title="预览节点"
        >
          <BaseIcon :path="ICONS.preview" className="w-5 h-5" />
        </button>

    <div class="grid grid-cols-2 gap-3">
      <button
        type="button"
        @click="emit('preview', profile)"
        class="flex items-center justify-center px-4 py-3 border border-primary-200 dark:border-primary-500/30 text-sm font-bold rounded-xl text-primary-700 dark:text-primary-300 bg-primary-50 dark:bg-primary-500/10 hover:bg-primary-100 dark:hover:bg-primary-500/20 transition-all active:scale-95 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-primary-500/50"
        aria-label="预览节点"
      >
        <BaseIcon :path="ICONS.preview" className="w-5 h-5 mr-1.5" />
        预览节点
      </button>
      <button
        type="button"
        @click="emit('copy-link', profile)"
        class="flex items-center justify-center px-4 py-3 border border-transparent text-sm font-bold rounded-xl text-white bg-gradient-to-r from-primary-600 to-indigo-600 hover:from-primary-500 hover:to-indigo-500 shadow-lg shadow-primary-500/30 transition-all hover:-translate-y-0.5 active:scale-95 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-primary-500/50"
        aria-label="复制链接"
      >
        <BaseIcon :path="ICONS.link" className="w-5 h-5 mr-1.5" />
        复制链接
      </button>
    </div>

    <QRCodeOverlay
      :profile="profile"
      :is-expanded="isQrExpanded"
      @close="emit('toggle-qr', profile)"
      @download="emit('download-qr', profile)"
      @register-canvas="(id, el) => emit('register-canvas', id, el)"
    />
  </div>
</template>
