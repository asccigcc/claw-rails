---
name: turbo
description: Apply when working with Turbo Drive, Frames, or Streams in Rails views (app/views/**/*.html.erb). Covers frame naming, lazy loading, and real-time updates.
---

# Turbo Standards

## Features
- **Turbo Drive** — navigation.
- **Turbo Frames** — partial page updates.
- **Turbo Streams** — real-time updates over WebSocket or after mutation.

## Best Practices
- Use semantic frame IDs (`"products-list"`, not `"frame1"`).
- Keep frame content focused on a single concern.
- Handle loading states explicitly.
- Mobile-first: test frame updates on small screens.

## Frame Example
```erb
<%# app/views/products/index.html.erb %>
<%= turbo_frame_tag "products-list" do %>
  <div class="space-y-4">
    <%= render ProductList::Component.new(products: @products) %>
  </div>

  <div data-controller="infinite-scroll">
    <%= turbo_frame_tag "products-pagination" do %>
      <%= render "pagination", pagy: @pagy %>
    <% end %>
  </div>
<% end %>
```

## Lazy-Loading Frames
Load UI on demand to improve initial page performance.

1. Replace eager-loaded components with lazy frames.
2. Define the endpoint that serves the frame content.
3. Render content inside a matching frame tag.

```erb
<%# Placeholder — loads src asynchronously %>
<%= turbo_frame_tag "component_#{id}", loading: :lazy, src: endpoint_path %>

<%# Endpoint response %>
<%= turbo_frame_tag "component_#{@id}" do %>
  <!-- component content -->
<% end %>
```
