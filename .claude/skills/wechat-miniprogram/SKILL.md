---
name: wechat-miniprogram
description: Develop WeChat mini-programs (微信小程序) from natural language descriptions. Activate when the user wants to create a WeChat mini-program, build a mini-app feature, or generate mini-program code. Simply describe the desired functionality and this skill will produce complete, runnable mini-program code with proper project structure, pages, components, API calls, and UI. Supports TDesign component library, Vant Weapp, and WeUI out of the box.
---

This skill transforms natural language descriptions into complete, working WeChat mini-program code. The user only needs to describe what they want to build.

## When to Activate

Activate when the user:
- Says "开发微信小程序", "create a WeChat mini-program", or similar
- Wants to build a mini-app feature with "小程序"
- Describes a use case that fits a mobile-first mini-program (shopping, booking, social, tools, etc.)
- Asks to generate mini-program code from a feature description

## Mini-Program Project Structure

A standard WeChat mini-program has this structure:

```
project-name/
├── app.js              # Application entry
├── app.json            # Global configuration
├── app.wxss            # Global styles
├── project.config.json # Devtool configuration
├── sitemap.json        # SEO configuration
└── pages/              # Page files
    ├── index/          # Each page
    │   ├── index.js
    │   ├── index.wxml
    │   ├── index.wxss
    │   └── index.json
    └── .../
└── components/         # Reusable components
    └── .../
```

## Generation Process

### Step 1: Analyze Requirements

Understand from the user's description:
- **Core functionality**: What does the mini-program do?
- **Key pages**: What screens are needed?
- **Data flow**: What API calls or local storage?
- **UI style**: TDesign, Vant, WeUI, or custom?
- **User interactions**: Forms, lists, navigation, modal dialogs?

### Step 2: Create Project Structure

Generate all necessary files:

```
app.js         → Application lifecycle, global state
app.json       → Pages, tabBar, window configuration
app.wxss       → Global styles, theme variables
pages/         → Each requested page
components/    → Reusable UI components
utils/         → Helper functions, API wrappers
```

### Step 3: Implement Pages

For each page, create the four files:

**.js** - Page logic, data, event handlers
```javascript
Page({
  data: { /* page state */ },
  onLoad(options) { /* initialization */ },
  // Event handlers
})
```

**.wxml** - Page template, using specified component library
```xml
<view class="container">
  <!-- UI structure -->
</view>
```

**.wxss** - Page-specific styles
```wxss
.container {
  padding: 32rpx;
}
```

**.json** - Page configuration, local component imports
```json
{
  "usingComponents": {},
  "navigationBarTitleText": "Page Title"
}
```

### Step 4: Add Component Library Support

If using TDesign:
```json
// app.json
{
  "usingComponents": {
    "t-button": "tdesign-miniprogram/button/button"
  }
}
```

If using Vant:
```json
{
  "usingComponents": {
    "van-button": "@vant/weapp/button/index"
  }
}
```

## Common Patterns

### Navigation
```javascript
// Jump to page
wx.navigateTo({ url: '/pages/detail/detail?id=1' })
// Switch tab
wx.switchTab({ url: '/pages/index/index' })
// Go back
wx.navigateBack()
```

### API Requests
```javascript
// Promise-based request wrapper
const request = (url, data) => {
  return new Promise((resolve, reject) => {
    wx.request({
      url: `https://api.example.com${url}`,
      method: 'POST',
      data,
      header: { 'Content-Type': 'application/json' },
      success: resolve,
      fail: reject
    })
  })
}
```

### Storage
```javascript
// Save
wx.setStorageSync('userInfo', userInfo)
// Read
const userInfo = wx.getStorageSync('userInfo')
// Remove
wx.removeStorageSync('userInfo')
```

### Form Handling
```javascript
// In wxml
<form bindsubmit="onSubmit">
  <input name="username" placeholder="用户名" />
  <button form-type="submit">提交</button>
</form>

// In js
onSubmit(e) {
  const { username } = e.detail.value
  // handle submit
}
```

### List Rendering
```xml
<block wx:for="{{items}}" wx:key="id">
  <view class="item">{{item.name}}</view>
</block>
```

### Conditional Rendering
```xml
<view wx:if="{{showEmpty}}">暂无数据</view>
<view wx:else>有数据</view>
```

## Output Format

Generate a complete mini-program project as an archive structure with all files clearly named and organized. Include:

1. **README.md** - Quick start guide with feature description
2. **app.js / app.json / app.wxss** - Application entry files
3. **pages/** - All pages with complete four-file structure
4. **components/** - Reusable components if needed
5. **utils/** - Helper functions and API wrappers
6. **project.config.json** - WeChat devtools configuration

## UI Component Recommendations

| Library | Best For | Import |
|---------|----------|--------|
| TDesign | Enterprise apps, clean modern UI | npm |
| Vant Weapp | E-commerce, familiar mobile patterns | npm |
| WeUI | Simple, native-feeling | npm |
| ColorUI | Beautiful, animated interfaces | Clone |

Default to TDesign for business apps, Vant for consumer apps unless user specifies otherwise.

## Quality Checklist

- [ ] All pages have complete four-file structure
- [ ] Navigation between pages works
- [ ] Forms handle input and submission correctly
- [ ] API calls include error handling
- [ ] Loading states shown during async operations
- [ ] Empty states handled for lists
- [ ] Styles use rpx for responsive sizing
- [ ] No hardcoded magic numbers - use CSS variables
- [ ] Safe area/padding for notched devices

## Example

**User input**: "开发一个待办事项小程序，可以添加、删除、完成待办事项，数据保存在本地"

**Generated output**: Complete mini-program with:
- `app.js/json/wxss` - Application files
- `pages/todos/todos.js/wxml/wxss/json` - Main todo page
- `components/todo-item/` - Todo item component
- `utils/storage.js` - Local storage helper
- Full CRUD functionality with checkbox completion
- Swipe-to-delete gesture support
