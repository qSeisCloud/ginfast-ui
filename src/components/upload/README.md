# 上传组件文档

## 组件概述

本项目提供了三种上传组件：
- `image-upload.vue` - 单图上传组件
- `multi-image-upload.vue` - 多图上传组件（照片墙形式）
- `file-upload.vue` - 通用文件上传组件

## 多图上传组件 (multi-image-upload.vue)

### 特性

- ✅ **照片墙形式** - 以网格布局展示已上传的图片
- ✅ **自定义上传请求** - 使用项目统一的API接口
- ✅ **双向绑定URL数组** - 支持v-model绑定图片URL数组
- ✅ **图片预览** - 点击图片可放大预览
- ✅ **删除功能** - 支持删除已上传的图片
- ✅ **上传进度显示** - 实时显示上传进度
- ✅ **数量限制** - 可配置最大上传数量
- ✅ **响应式设计** - 自适应不同屏幕尺寸

### 安装和使用

```vue
<template>
  <div>
    <!-- 基本用法 -->
    <multi-image-upload v-model="imageUrls" />
    
    <!-- 自定义配置 -->
    <multi-image-upload 
      v-model="productImages"
      title="上传商品图片"
      :width="120"
      :height="120"
      :max-count="5"
      accept=".jpg,.jpeg,.png,.gif"
    />
  </div>
</template>

<script setup lang="ts">
import MultiImageUpload from '@/components/upload/multi-image-upload.vue';

const imageUrls = ref<string[]>([]);
const productImages = ref<string[]>([]);
</script>
```

### Props 属性

| 属性名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| modelValue | string[] | [] | 双向绑定的图片URL数组 |
| title | string | '上传图片' | 上传按钮的标题文字 |
| accept | string | 'image/*' | 接受的文件类型 |
| width | string \| number | 100 | 图片显示宽度 |
| height | string \| number | 100 | 图片显示高度 |
| maxCount | number | 10 | 最大上传数量限制 |

### 事件

| 事件名 | 说明 | 参数 |
|--------|------|------|
| update:modelValue | 图片URL数组变化时触发 | (urls: string[]) |

### 功能说明

#### 1. 照片墙布局
- 已上传的图片以网格形式排列
- 支持鼠标悬停效果
- 响应式布局，自动换行

#### 2. 图片操作
- **预览**: 点击图片可放大预览
- **删除**: 点击删除按钮可移除图片
- **上传进度**: 上传过程中显示进度条

#### 3. 上传限制
- 支持配置最大上传数量
- 达到上限后上传按钮自动禁用
- 显示当前上传数量/最大数量

#### 4. 自定义上传
- 使用项目统一的 `uploadAffixAPI` 接口
- 支持FormData格式上传
- 自动处理上传成功/失败状态

### 样式定制

组件使用CSS变量，可以通过以下方式自定义样式：

```css
/* 修改主题色 */
:root {
  --color-primary: #1890ff;
  --color-primary-light-1: #e6f7ff;
}

/* 自定义图片尺寸 */
.custom-upload {
  --upload-width: 120px;
  --upload-height: 120px;
}
```

### 注意事项

1. **URL处理**: 组件会自动调用 `handleUrl` 函数处理相对路径
2. **文件类型**: 默认接受所有图片类型，可通过 `accept` 属性限制
3. **上传状态**: 上传失败的文件会显示错误状态，可以重新上传
4. **性能优化**: 建议设置合理的 `maxCount` 避免上传过多图片

### 示例

查看 `multi-image-upload-demo.vue` 文件获取完整的使用示例。

## 单图上传组件 (image-upload.vue)

### 基本用法

```vue
<template>
  <image-upload v-model="avatarUrl" title="上传头像" />
</template>

<script setup lang="ts">
import ImageUpload from '@/components/upload/image-upload.vue';

const avatarUrl = ref('');
</script>
```

### Props 属性

| 属性名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| modelValue | string | '' | 双向绑定的图片URL |
| title | string | 'Upload' | 上传按钮的标题文字 |
| accept | string | 'image/*' | 接受的文件类型 |
| width | string \| number | 120 | 图片显示宽度 |
| height | string \| number | 120 | 图片显示高度 |

## 通用文件上传组件 (file-upload.vue)

### 特性

- ✅ **任意文件上传** - 支持上传任意类型的文件
- ✅ **文件后缀限制** - 可通过 `accept` 属性限制文件类型
- ✅ **数量限制** - 可配置最大上传数量
- ✅ **双向绑定JSON数组** - 支持v-model绑定JSON数组字符串
- ✅ **文件列表展示** - 清晰展示已上传文件信息
- ✅ **上传进度显示** - 实时显示上传进度
- ✅ **删除功能** - 支持删除已上传的文件
- ✅ **重试功能** - 上传失败可重试
- ✅ **下载功能** - 支持下载已上传的文件

### 安装和使用

```vue
<template>
  <div>
    <!-- 基本用法 - 上传任意文件 -->
    <file-upload v-model="files" />
    
    <!-- 限制文件类型和数量 -->
    <file-upload
      v-model="documents"
      title="上传文档"
      accept=".pdf,.doc,.docx,.xls,.xlsx"
      :max-count="5"
    />
    
    <!-- 上传图片 -->
    <file-upload
      v-model="images"
      title="上传图片"
      accept="image/*"
      :max-count="10"
    />
  </div>
</template>

<script setup lang="ts">
import FileUpload from '@/components/upload/file-upload.vue';

// 双向绑定的值为JSON数组字符串
const files = ref<string>('[]');
const documents = ref<string>('[]');
const images = ref<string>('[]');

// 监听变化
watch(() => files.value, (newVal) => {
  const fileList = JSON.parse(newVal);
  console.log('文件列表:', fileList);
  // fileList 格式:
  // [
  //   {
  //     id: 1,
  //     name: "example.pdf",
  //     size: 1024000,
  //     url: "https://example.com/file.pdf",
  //     suffix: ".pdf",
  //     ftype: "document"
  //   }
  // ]
});
</script>
```

### Props 属性

| 属性名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| modelValue | string | '[]' | 双向绑定的文件信息JSON数组字符串 |
| title | string | '上传文件' | 上传按钮的标题文字 |
| accept | string | '*' | 接受的文件类型，如 '.pdf,.doc' 或 'image/*' |
| maxCount | number | 10 | 最大上传数量限制 |

### Events 事件

| 事件名 | 说明 | 参数 |
|--------|------|------|
| update:modelValue | 文件列表变化时触发 | (jsonString: string) |
| change | 文件状态变化时触发 | (fileList: FileInfo[]) |
| success | 上传成功时触发 | (fileData: AffixItem) |
| error | 上传失败时触发 | (error: Error) |

### 文件信息格式

双向绑定的值为 JSON 数组字符串，每个文件对象包含以下信息：

```typescript
interface FileInfo {
    id: number;        // 文件ID
    name: string;      // 文件名
    size: number;      // 文件大小（字节）
    url: string;       // 文件URL
    suffix: string;    // 文件后缀
    ftype: string;     // 文件类型
}
```

### 功能说明

#### 1. 文件类型限制

通过 `accept` 属性限制可上传的文件类型：

```vue
<!-- 限制为PDF和Word文档 -->
<file-upload v-model="files" accept=".pdf,.doc,.docx" />

<!-- 限制为所有图片 -->
<file-upload v-model="files" accept="image/*" />

<!-- 限制为特定类型 -->
<file-upload v-model="files" accept=".txt,.csv,.json" />
```

#### 2. 数量限制

通过 `maxCount` 属性限制最大上传数量：

```vue
<!-- 最多上传5个文件 -->
<file-upload v-model="files" :max-count="5" />
```

#### 3. 文件操作

- **预览**: 文件列表显示文件名、大小和上传状态
- **下载**: 点击下载按钮可下载已上传的文件
- **删除**: 点击删除按钮可移除文件
- **重试**: 上传失败的文件可点击重试

#### 4. 上传状态

- `已上传` - 文件上传成功
- `上传中` - 文件正在上传，显示进度百分比
- `上传失败` - 文件上传失败，可重试

### 示例场景

#### 场景1：上传用户身份证

```vue
<template>
  <file-upload
    v-model="idCardFiles"
    title="上传身份证"
    accept=".jpg,.jpeg,.png"
    :max-count="2"
  />
</template>

<script setup lang="ts">
import FileUpload from '@/components/upload/file-upload.vue';

const idCardFiles = ref<string>('[]');
</script>
```

#### 场景2：上传合同文档

```vue
<template>
  <file-upload
    v-model="contractFiles"
    title="上传合同"
    accept=".pdf,.doc,.docx"
    :max-count="10"
  />
</template>

<script setup lang="ts">
import FileUpload from '@/components/upload/file-upload.vue';

const contractFiles = ref<string>('[]');
</script>
```

### 注意事项

1. **JSON格式**: `modelValue` 是一个 JSON 数组字符串，使用前需要 `JSON.parse()` 解析
2. **文件类型**: 默认接受所有文件类型，建议通过 `accept` 属性明确限制
3. **大小限制**: 组件本身不限制文件大小，如需限制请后端配置
4. **URL处理**: 上传成功后返回的 URL 可直接用于下载或预览

## API 依赖

三个组件都依赖以下API和工具函数：

- `uploadAffixAPI` - 文件上传接口
- `handleUrl` - URL处理函数（图片组件使用）
- `Message` - 消息提示组件

确保这些依赖在项目中正确配置。