
Let me explain how to implement script injection functionality for your VOA Hotels system.

Core Approach:
1. Create a new package `voa-script` that will generate a minified, self-contained JavaScript bundle
2. This bundle will:
   - Load required dependencies
   - Initialize VOA components
   - Mount them into a shadow DOM for style isolation
   - Expose a global configuration API

Key Technical Considerations:

1. **Bundle Size Optimization**
- Tree-shaking to only include used components
- Code splitting for lazy loading less critical features
- Compression and minification

2. **Shadow DOM & Style Isolation**
- Prevents CSS conflicts with host page
- Encapsulates VOA components
- Handle font loading and assets within shadow root

3. **Script Loading Strategy**
```js
// Client usage will be as simple as:
<script>
  (function(w,d,s,o,f,js,fjs){
    w['VOAWidget']=o;
    w[o]=w[o]||function(){(w[o].q=w[o].q||[]).push(arguments)};
    js=d.createElement(s),fjs=d.getElementsByTagName(s)[0];
    js.id='voa-widget';js.src=f;js.async=1;
    fjs.parentNode.insertBefore(js,fjs);
  }(window,document,'script','voa','https://cdn.voa-hotels.com/widget.js'));
  
  voa('init', { apiKey: 'xxx' });
</script>
```

4. **Configuration API**
- Global namespace management
- Event system for widget lifecycle
- Error handling and logging
- Configuration validation

Things to Learn/Research:

1. **Build Tools**
- Rollup/webpack for IIFE bundles
- Code splitting strategies
- Source maps for production debugging

2. **Web Components**
- Shadow DOM APIs
- Custom Elements
- HTML Templates

3. **Performance**
- Resource loading strategies
- Browser paint optimization
- Memory management

4. **Security**
- CSP compatibility
- XSS prevention
- Iframe sandboxing (if needed)

5. **Testing**
- Cross-browser testing
- Integration testing
- Performance benchmarking

Would you like me to start setting up the basic structure for this new package? We can begin with either:
1. Basic script injection setup
2. Shadow DOM implementation
3. Build configuration


# CSS Configuration

```typescript
css: {
  modules: {
    localsConvention: 'camelCase',
    scopeBehaviour: 'local'
  }
}
```

This means we can skip the Shadow DOM complexity since your styles are already modular and won't conflict with the host page. The build is also already set up to generate both ES and UMD formats.

For the script injection approach, we can:

1. Create a new package `voa-script` that will:
   - Import the UMD build from `voa-hotels`
   - Handle initialization and configuration
   - Provide the global API

2. Modify the current build slightly to:
   - Ensure CSS is bundled into a single file
   - Add compression for production builds
   - Generate a smaller entry point for the script version

Would you like me to:
1. Create the new `voa-script` package structure
2. Modify the current build config to better support script injection
3. Show you a sample implementation of the script loader?

Let me know which part you want to tackle first.


















Challenges:
- [ ] fast release cycle
- [ ] easy configuration for different clients
- [ ] easy to manage its deployment


