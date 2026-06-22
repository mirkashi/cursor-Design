## Integrate the <BlobCursor /> component from React Bits

You are helping integrate an open-source React component into an existing application.

### Component: BlobCursor
### Variant: JavaScript + CSS
### Dependencies: gsap

---

### Usage Example
```jsx
import BlobCursor from './BlobCursor';

<BlobCursor
  blobType="circle"
  fillColor="#5227FF"
  trailCount={3}
  sizes={[60, 125, 75]}
  innerSizes={[20, 35, 25]}
  innerColor="rgba(255,255,255,0.8)"
  opacities={[0.6, 0.6, 0.6]}
  shadowColor="rgba(0,0,0,0.75)"
  shadowBlur={5}
  shadowOffsetX={10}
  shadowOffsetY={10}
  filterStdDeviation={30}
  useFilter={true}
  fastDuration={0.1}
  slowDuration={0.5}
  zIndex={100}
/>
```

### Props
| Prop | Type | Default | Description |
|------|------|---------|-------------|
| blobType | 'circle' | 'square' | 'circle' | Shape of the blobs. |
| fillColor | string | '#5227FF' | Background color of each blob. |
| trailCount | number | 3 | How many trailing blobs. |
| sizes | number[] | [60, 125, 75] | Sizes (px) of each blob. Length must be ≥ trailCount. |
| innerSizes | number[] | [20, 35, 25] | Sizes (px) of inner dots. Length must be ≥ trailCount. |
| innerColor | string | 'rgba(255,255,255,0.8)' | Background color of the inner dot. |
| opacities | number[] | [0.6, 0.6, 0.6] | Opacity of each blob. Length ≥ trailCount. |
| shadowColor | string | 'rgba(0,0,0,0.75)' | Box-shadow color. |
| shadowBlur | number | 5 | Box-shadow blur radius (px). |
| shadowOffsetX | number | 10 | Box-shadow X offset (px). |
| shadowOffsetY | number | 10 | Box-shadow Y offset (px). |
| filterId | string | 'blob' | Optional custom filter ID (for multiple instances). |
| filterStdDeviation | number | 30 | feGaussianBlur stdDeviation for SVG filter. |
| filterColorMatrixValues | string | '1 0 0 ...' | feColorMatrix values for SVG filter. |
| useFilter | boolean | true | Enable the SVG filter. |
| fastDuration | number | 0.1 | GSAP duration for the lead blob. |
| slowDuration | number | 0.5 | GSAP duration for the following blobs. |
| fastEase | string | 'power3.out' | GSAP ease for the lead blob. |
| slowEase | string | 'power1.out' | GSAP ease for the following blobs. |
| zIndex | number | 100 | CSS z-index of the whole component. |

### Full Component Source
```jsx
'use client';

import { useRef, useEffect, useCallback } from 'react';
import gsap from 'gsap';
import './BlobCursor.css';

export default function BlobCursor({
  blobType = 'circle',
  fillColor = '#5227FF',
  trailCount = 3,
  sizes = [60, 125, 75],
  innerSizes = [20, 35, 25],
  innerColor = 'rgba(255,255,255,0.8)',
  opacities = [0.6, 0.6, 0.6],
  shadowColor = 'rgba(0,0,0,0.75)',
  shadowBlur = 5,
  shadowOffsetX = 10,
  shadowOffsetY = 10,
  filterId = 'blob',
  filterStdDeviation = 30,
  filterColorMatrixValues = '1 0 0 0 0 0 1 0 0 0 0 0 1 0 0 0 0 0 35 -10',
  useFilter = true,
  fastDuration = 0.1,
  slowDuration = 0.5,
  fastEase = 'power3.out',
  slowEase = 'power1.out',
  zIndex = 100
}) {
  const containerRef = useRef(null);
  const blobsRef = useRef([]);

  const updateOffset = useCallback(() => {
    if (!containerRef.current) return { left: 0, top: 0 };
    const rect = containerRef.current.getBoundingClientRect();
    return { left: rect.left, top: rect.top };
  }, []);

  const handleMove = useCallback(
    e => {
      const { left, top } = updateOffset();
      const x = 'clientX' in e ? e.clientX : e.touches[0].clientX;
      const y = 'clientY' in e ? e.clientY : e.touches[0].clientY;

      blobsRef.current.forEach((el, i) => {
        if (!el) return;
        const isLead = i === 0;
        gsap.to(el, {
          x: x - left,
          y: y - top,
          duration: isLead ? fastDuration : slowDuration,
          ease: isLead ? fastEase : slowEase
        });
      });
    },
    [updateOffset, fastDuration, slowDuration, fastEase, slowEase]
  );

  useEffect(() => {
    const onResize = () => updateOffset();
    window.addEventListener('resize', onResize);
    return () => window.removeEventListener('resize', onResize);
  }, [updateOffset]);

  return (
    <div
      ref={containerRef}
      className="blob-container"
      style={{ zIndex }}
      onMouseMove={handleMove}
      onTouchMove={handleMove}
    >
      {useFilter && (
        <svg style={{ position: 'absolute', width: 0, height: 0 }}>
          <filter id={filterId}>
            <feGaussianBlur in="SourceGraphic" result="blur" stdDeviation={filterStdDeviation} />
            <feColorMatrix in="blur" values={filterColorMatrixValues} />
          </filter>
        </svg>
      )}

      <div className="blob-main" style={{ filter: useFilter ? `url(#${filterId})` : undefined }}>
        {Array.from({ length: trailCount }).map((_, i) => (
          <div
            key={i}
            ref={el => (blobsRef.current[i] = el)}
            className="blob"
            style={{
              width: sizes[i],
              height: sizes[i],
              borderRadius: blobType === 'circle' ? '50%' : '0%',
              backgroundColor: fillColor,
              opacity: opacities[i],
              boxShadow: `${shadowOffsetX}px ${shadowOffsetY}px ${shadowBlur}px 0 ${shadowColor}`
            }}
          >
            <div
              className="inner-dot"
              style={{
                width: innerSizes[i],
                height: innerSizes[i],
                top: (sizes[i] - innerSizes[i]) / 2,
                left: (sizes[i] - innerSizes[i]) / 2,
                backgroundColor: innerColor,
                borderRadius: blobType === 'circle' ? '50%' : '0%'
              }}
            />
          </div>
        ))}
      </div>
    </div>
  );
}

```

### Component CSS
```css
.blob-container {
  position: relative;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.blob-main {
  pointer-events: none;
  position: absolute;
  width: 100%;
  height: 100%;
  overflow: hidden;
  background: transparent;
  user-select: none;
  cursor: default;
}

.blob {
  position: absolute;
  will-change: transform;
  transform: translate(-50%, -50%);
}

.inner-dot {
  position: absolute;
}

```

### Integration Instructions
1. Install any listed dependencies.
2. Copy the component source into the appropriate directory in the project.
3. Import the CSS file alongside the component.
4. Import and render the component using the usage example above as a starting point.
5. Adjust props as needed for the specific use case — refer to the props table for all available options.
