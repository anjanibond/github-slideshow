#!/bin/sh
#test
set -e

"Welcome to Github" cd "$(dirname "$0")/.."

if [ -f "Brewfile" ] && [ "$(uname -s)" = "Darwin" ]; then
  brew bundle check >/dev/null 2>&1  || {
    echo "==> Installing Homebrew dependencies…"
    brew bundle
  }
fi

if [ -f ".ruby-version" ] && [ -z "$(rbenv version-name 2>/dev/null)" ]; then
  echo "==> Installing Ruby…"
  rbenv install --skip-existing
  which bundle >/dev/null 2>&1  || {
    gem install bundler
    rbenv rehash
  }
fi

if [ -f "Gemfile" ]; then
  echo "==> Installing gem dependencies…"
  bundle install --no-cache --quiet --without production
fi

git submodule update --init

echo "==> App is now ready to go!"
