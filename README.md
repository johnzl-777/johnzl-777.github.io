# John Long's blog

Built on top of the [sidey](https://github.com/ronv/sidey) theme with components from [Chris Schuld's version](https://github.com/cbschuld/chrisschuld.com) of the Hyde Jekyll Template.

Fonts from [Google Fonts](https://fonts.google.com/), icons from [Font Awesome](https://fontawesome.com/), and button hover effects from [Hover.css](https://ianlunn.github.io/Hover/)

## Theme Template

If you're interested in just the sidey theme with the stuff I've added/hybridized on top of it, you can check out [H+](https://github.com/johnzl-777/H-Plus)

## Local Development - Live
```
docker run --env=DEBUG=true --rm --volume="$PWD:/srv/jekyll" -p 3000:4000 -it jekyll/jekyll:3.8 jekyll serve -w --force_polling
```

## Local Development - Build for Deployment
```
docker run --env=DEBUG=true --env JEKYLL_ENV=development --rm --volume="$PWD:/srv/jekyll" -p 3000:4000 -it jekyll/jekyll:3.8 jekyll build
```

## License
Open sourced under the [MIT License](LICENSE.md)
