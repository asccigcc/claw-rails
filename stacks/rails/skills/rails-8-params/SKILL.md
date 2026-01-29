---
name: rails-8-params
description: Apply when editing Rails 8+ controllers. Covers `params.expect` strong-parameter API. Verify version in Gemfile.lock first; skip on Rails 7.
---

# Rails 8 Strong Parameters (`params.expect`)

Rails 8 introduced `params.expect` as the preferred strong-parameters API. It enforces both presence of the root key and the shape of permitted attributes in one call, raising `ActionController::ParameterMissing` on mismatch.

## Pattern

```ruby
class PostsController < ApplicationController
  def create
    @post = Post.create!(post_params)
    redirect_to @post
  end

  private

  def post_params
    params.expect(post: [:title, :body, :published, tag_ids: []])
  end
end
```

## Nested resources

```ruby
params.expect(post: [:title, comments_attributes: [[:body, :author_id]]])
```

The double-bracket `[[:body, :author_id]]` signals an array of hashes.

## Migration from `require.permit`

Old (still works, but not idiomatic in Rails 8):
```ruby
params.require(:post).permit(:title, :body)
```

New:
```ruby
params.expect(post: [:title, :body])
```

## When to stay on `require.permit`
- Params without a wrapping root key (rare in REST-compliant apps).
- Dynamic permit lists computed at runtime.

## Verification
Before generating code, confirm Rails version via `Gemfile.lock`. If `rails (8.x.x)` is present, use `expect`. On Rails 7 or unknown, hand off to `rails-7-params`.
