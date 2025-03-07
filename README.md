<br><br>

![readme-header](https://pdfslick.dev/pdfslick_logo.svg)

<br><br>

<div align="center">
عرض ملفات PDF والتفاعل معها في تطبيقات React و SolidJS و Svelte و JavaScript
<br><br>

[![Actions Status](https://github.com/pdfslick/pdfslick/actions/workflows/publish_site.yml/badge.svg)](https://github.com/pdfslick/pdfslick/actions)
[![GitHub release](https://img.shields.io/github/release/pdfslick/pdfslick.svg)](https://github.com/pdfslick/pdfslick/)
[![npm version](https://img.shields.io/npm/v/@pdfslick/core.svg)](https://www.npmjs.com/package/@pdfslick/core)
[![npm](https://img.shields.io/npm/dt/@pdfslick/core.svg)](https://www.npmjs.com/package/@pdfslick/core)

<!-- [![npm](https://img.shields.io/npm/dt/@pdfslick/react.svg)](https://www.npmjs.com/package/@pdfslick/react)
[![npm](https://img.shields.io/npm/dt/@pdfslick/solid.svg)](https://www.npmjs.com/package/@pdfslick/solid) -->

<br>

[البدء](https://pdfslick.dev/docs) | [أمثلة](https://pdfslick.dev/examples)
<br><br>

</div>

---

<br>

PDFSlick هي مكتبة تمكن من عرض والتفاعل مع مستندات PDF في تطبيقات React و SolidJS و Svelte و JavaScript.
تم بناؤها على أساس [PDF.js](https://github.com/mozilla/pdf.js) من Mozilla، وتستخدم [Zustand](https://github.com/pmndrs/zustand) لتوفير مخزن تفاعلي للمستندات المحملة.

## PDFSlick لـ React

للبدء مع React، قم بتشغيل:

```shell
npm install @pdfslick/react
# yarn add @pdfslick/react
# pnpm add @pdfslick/react
```

يمكنك البدء في استخدام PDFSlick باستخدام الخطاف `usePDFSlick()`، كما هو الحال مع المثال الأساسي التالي:

```jsx
import { usePDFSlick } from "@pdfslick/react";
import PDFNavigation from "./yourcomponents/PDFNavigation";

//
// من الضروري تضمين أنماط CSS الخاصة بـ PDFSlick بمجرد أن تتمكن من القيام بذلك في ملف `App.tsx` الرئيسي على سبيل المثال
//
import "@pdfslick/react/dist/pdf_viewer.css";

type PDFViewerComponentProps = {
  pdfFilePath: string,
};

const PDFViewerComponent = ({ pdfFilePath }: PDFViewerComponent) => {
  const { viewerRef, usePDFSlickStore, PDFSlickViewer } = usePDFSlick(
    pdfFilePath,
    {
      scaleValue: "page-fit",
    }
  );

  /*
   يمكنك الوصول إلى المتجر باستخدام هوك `usePDFSlickStore()` — يمكنك تمرير is
كدعامة إلى مكونات أخرى (مثل `<PDFNavigation />` أدناه)
أشرطة الأدوات، والأشرطة الجانبية، والمكونات التي تعرض الصور المصغرة وما إلى ذلك.
واستخدامها هنا للحصول على خصائص وتعديلات مستندات PDF والعارض والتفاعل معها
   */
  const scale = usePDFSlickStore((s) => s.scale);
  const numPages = usePDFSlickStore((s) => s.numPages);
  const pageNumber = usePDFSlickStore((s) => s.pageNumber);

  return (
    <div className="absolute inset-0 pdfSlick">
      <div className="relative h-full">
        <PDFSlickViewer {...{ viewerRef, usePDFSlickStore }} />

        {/*
          قم بتمرير متجر PDFSlick إلى مكوناتك المخصصة
        */}
        <PDFNavigation {...{ usePDFSlickStore }} />

        {/*
          يتم تحديث قيم متجر PDFSlick تلقائيًا
        */}
        <div className="absolute w-full top-0 left-0">
          <p>Current scale: {scale}</p>
          <p>Current page number: {pageNumber}</p>
          <p>Total number of pages: {numPages}</p>
        </div>
      </div>
    </div>
  );
};

export default PDFViewerComponent;
```

مع مسار مستند PDF وكائن خيارات PDFSlick، يقوم الخطاف `usePDFSlick()` بإرجاع كائن يتكون (من بين أشياء أخرى) من:

- `PDFSlickViewer` هو مكون عارض PDF المستخدم لعرض مستند PDF
- `viewerRef` هو استدعاء `ref` المقدم كدعامة لمكون `<PDFSlickViewer />`
- `usePDFSlickStore` يمكّن من استخدام متجر PDFSlick داخل مكونات React الخاصة بك

<br>

[More on using PDFSlick with React](https://pdfslick.dev/docs/react) | [Checkout the React Examples](./apps/web/examples)

<br>

## PDFSlick لـ SolidJS

للبدء مع PDFSlick لـ SolidJS، قم بتشغيل:

```shell
npm install @pdfslick/solid
# yarn add @pdfslick/solid
# pnpm add @pdfslick/solid
```

يمكنك البدء في استخدام PDFSlick باستخدام الخطاف `usePDFSlick()`، كما هو الحال مع المثال الأساسي التالي:

```jsx
import { Component, createSignal } from "solid-js";
import { usePDFSlick } from "@pdfslick/solid";
import PDFNavigation from "./yourcomponents/PDFNavigation";

//
// من الضروري تضمين أنماط CSS الخاصة بـ PDFSlick بمجرد أن تتمكن من القيام بذلك في ملف `App.tsx` الرئيسي على سبيل المثال
//
import "@pdfslick/solid/dist/pdf_viewer.css";

type PDFViewerComponentProps = {
  pdfFilePath: string,
};

const PDFViewerComponent: Component<PDFViewerComponentProps> = ({
  pdfFilePath,
}) => {
  const {
    viewerRef,
    pdfSlickStore: store,
    PDFSlickViewer,
  } = usePDFSlick(pdfFilePath);

  return (
    <div class="absolute inset-0 flex flex-col pdfSlick">
      <div class="flex-1 relative h-full">
        <PDFSlickViewer {...{ store, viewerRef }} />

        {/*
          Pass PDFSlick's store to your custom components (like the `<PDFNavigation />` below) —
          Toolbars, Sidebars, components which render thumbnails etc.
          and use it as here to get and react on 
          PDF document and viewer properties and changes
        */}
        <PDFNavigation {...{ store }} />

        {/*
          PDFSlick's store values automatically update
        */}
        <div className="absolute w-full top-0 left-0">
          <p>Current scale: {store.scale}</p>
          <p>Current page number: {store.pageNumber}</p>
          <p>Total number of pages: {store.numPages}</p>
        </div>
      </div>
    </div>
  );
};

export default PDFViewerComponent;
```

Provided with the PDF Document path and options object, the `usePDFSlick()` hook returns an object consisting (among the other things) of:

- `PDFSlickViewer` is the PDF Viewer component used for viewing the PDF document
- `viewerRef` is the `ref` callback that is provided as a prop to the `<PDFSlickViewer />` component
- `pdfSlickStore` is the PDFSlick store

<br>

[More on using PDFSlick with Solid](https://pdfslick.dev/docs/solid) | [Checkout the SolidJS Examples](./apps/solidweb/src/examples)

<br>

## PDFSlick لـ Svelte

للبدء مع PDFSlick لـ Svelte، قم بتشغيل:

```shell
npm install @pdfslick/core
# yarn add @pdfslick/core
# pnpm add @pdfslick/core
```

You can load a PDF document and subscribe to a portion of or the entire PDFSlick store's state, like in the following basic example:

```html
<script lang="ts">
  import type { PDFSlick } from '@pdfslick/core';
  import { onMount, onDestroy } from 'svelte';

  // ...

  /**
   * مرجع إلى حاوية عارض PDF
   */
  let container: HTMLDivElement;

  /**
   * مرجع إلى مثيل pdfSlick
   */
  let pdfSlick: PDFSlick;

  /**
   * احتفظ بأجزاء حالة PDF Slick التي نرغب في استخدامها في تطبيقك
   */
  let pageNumber = 1;
  let numPages = 0;

  onMount(async () => {
    /**
     * يحدث كل هذا على جانب العميل، لذا سنتأكد من تحميله هناك فقط
     */
    const { create, PDFSlick } = await import('@pdfslick/core');

    /**
     * إنشاء متجر PDF Slick
     */
    const store = create();

    pdfSlick = new PDFSlick({
      container,
      store,
      options: {
        scaleValue: 'page-fit'
      }
    });

    /**
     * تحميل مستند PDF
     */
    pdfSlick.loadDocument(url);
    store.setState({ pdfSlick });

    /**
     الاشتراك في تغييرات الحالة، والاحتفاظ بقيم الاهتمام كمتغيرات Svelte تفاعلية، (أو بدلاً من ذلك يمكننا ربط هذه الحالة أو حالة PDF بأكملها بمتجر Svelte)
     
     احتفظ أيضًا بمرجع لوظيفة إلغاء الاشتراك التي نستدعيها لتدمير المكون
     */
    unsubscribe = store.subscribe((s) => {
      pageNumber = s.pageNumber;
      numPages = s.numPages;
    });
  });

  onDestroy(() => unsubscribe());

	// ...
</script>

<!-- ... -->

<div class="absolute inset-0 bg-slate-200/70 pdfSlick">

  <div class="flex-1 relative h-full" id="container">
    <!--
      الجزء المهم - نستخدم المرجع إلى هذه "الحاوية" عند إنشاء مثيل PDF Slick أعلاه
    -->
    <div id="viewerContainer" class="pdfSlickContainer absolute inset-0 overflow-auto" bind:this={container}>
      <div id="viewer" class="pdfSlickViewer pdfViewer"></div>
    </div>
  </div>

  <!-- ... -->

  <!-- استخدم `pdfSlick` و`pageNumber` و`numPages` لإنشاء ترقيم صفحات PDF -->
  <div class="flex justify-center">
    <button
      on:click={() => pdfSlick?.gotoPage(Math.max(pageNumber - 1, 1))}
      disabled={pageNumber <= 1}
		>
      إظهار الصفحة السابقة
    </button>
    <button
      on:click={() =>  pdfSlick?.gotoPage(Math.min(pageNumber + 1, numPages))}
      disabled={pageNumber >= numPages}
    >
      إظهار الصفحة التالية
    </button>
  </div>

</div>

<!-- ... -->
```

<br>

[المزيد حول استخدام PDFSlick مع Svelte](https://pdfslick.dev/docs/svelte) | [قم بإلقاء نظرة على أمثلة Svelte](./apps/svelteweb/src)

<br>

## الدافع

[PDF.js](https://github.com/mozilla/pdf.js) هو برنامج رائع. كما أنه مستقر وناضج - فهو يشغل عارض PDF في Mozilla Firefox وموجود منذ عام 2011. ومع ذلك، فهو مكتوب بـ JavaScript الأساسي، وعندما يتعلق الأمر باستخدامه مع مكتبات مثل React و SolidJS (على الرغم من إمكانية ذلك) فإنه صعب قليلاً من حيث دمجه في هذه البيئات القائمة على المكونات والتفاعلية. يحاول PDFSlick تغليف كل هذه الوظائف الرائعة في شكل يسهل دمجه في عالم React و SolidJS - كمكونات ومخزن تفاعلي.

## المفاهيم الأساسية

جوهر PDFSlick موجود في حزمة `@pdfslick/core`. إنها تغلف وظائف `PDF.js` وتربطها بالمخزن. حزمة `@pdfslick/core` هذه هي الأساس لحزم React و SolidJS، والتي تقوم بتحويل المخزن وجعله مناسبًا للمكتبة المناسبة، بالإضافة إلى توفير مكونات لعارض PDF والصور المصغرة.

اعتمادًا على احتياجاتك، قد تجد أنه من المفيد في هذه المرحلة مواصلة التعرف على المزيد حول استخدام PDFSlick مع React و SolidJS على التوالي. ومع ذلك، إذا كنت مهتمًا حقًا، يمكنك معرفة المزيد حول استخدام حزمة `@pdfslick/core` الخاصة بـ PDFSlick مع تطبيقات JavaScript الأساسية ومع المكتبات الأخرى غير React و SolidJS في الأقسام أدناه.

<br>

[تعرف على المزيد عن PDFSlick](https://pdfslick.dev) | [تحقق من الأمثلة](https://pdfslick.dev/examples)

## شكر وتقدير

- [Vane Kosturanov](https://kosturanov.com/portfolio/logo-branding-design) لتصميم الشعار
- [VS Code Icons](https://github.com/microsoft/vscode-codicons) للأيقونات المستخدمة في جميع الأمثلة
