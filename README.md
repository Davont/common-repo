# common-repo

import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue';
import path from 'path';
import fs from 'fs';

// 动态生成入口
const modulesDir = path.resolve(__dirname, 'core/src');
const input = {};
fs.readdirSync(modulesDir).forEach(dir => {
  const fullPath = path.resolve(modulesDir, dir, 'index.ts');
  if (fs.existsSync(fullPath)) {
    input[dir] = fullPath;
  }
});

export default defineConfig({
  plugins: [vue()],
  build: {
    rollupOptions: {
      input,
      output: {
        preserveModules: true,
        preserveModulesRoot: 'core/src',
        entryFileNames: '[name]/index.js',
        dir: 'dist/core'
      },
      external: ['vue']
    },
  }
});
