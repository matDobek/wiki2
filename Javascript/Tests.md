### setup

```bash
npm install -D vitest
npm install -D jsdom
```

### tldr

```js
// src/utils/math.js
// src/utils/__tests__/math.test.js
import { describe, it, expect } from 'vitest';
import { randInSphere } from '../math';

describe('randInSphere', () => {
  it('returns a point within the given radius', () => {
    // ...
    expect(distance).toBeLessThanOrEqual(radius);
  });
});

```

```js
// vite.config.js
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  test: {
    globals: true, // describe, it, expect globally
    environment: 'jsdom', // virtual dom in test
  },
});
```
