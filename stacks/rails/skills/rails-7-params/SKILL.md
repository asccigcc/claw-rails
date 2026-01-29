---
name: rails-7-params
description: Apply when editing Rails 7 controllers. Covers `params.require(...).permit(...)` strong-parameter API. Verify version in Gemfile.lock first; skip on Rails 8+.
---

# Rails 7 Strong Parameters (`params.require.permit`)

Rails 7 controllers use the classic `require` + `permit` pair. `require` asserts presence of a root key; `permit` whitelists attributes.

## Pattern

```ruby
class PostsController < ApplicationController
  def create
    @post = Post.create!(post_params)
    redirect_to @post
  end

  private

  def post_params
    params.require(:post).permit(:title, :body, :published, tag_ids: [])
  end
end
```

## Nested resources

```ruby
params.require(:post).permit(
  :title,
  comments_attributes: [:id, :body, :author_id, :_destroy]
)
```

## Dynamic permit lists

```ruby
def permitted_attributes
  attrs = [:title, :body]
  attrs << :published if current_user.admin?
  attrs
end

def post_params
  params.require(:post).permit(permitted_attributes)
end
```

## Verification
Before generating code, confirm Rails version via `Gemfile.lock`. If `rails (7.x.x)` is present, use `require.permit`. On Rails 8+, hand off to `rails-8-params`.
