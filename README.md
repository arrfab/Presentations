# Presentations
Slides I'll use for online presentations, based on RemarkJS

## Writing presenations
Just create a .md file (in markdown format), following the [RemarkJS](https://github.com/gnab/remark/wiki) methodology to seperate slides, etc ...
You don't need html code, just the .md content, as we'll concatenate everything in next step

## Rendering presentations
Once you're happy with content, just call this script that will output html file, based from templates files
```
./render_preso loadays-2019-ssh-tips.md 
rendering RemarkJS html output file to output/loadays-2019-ssh-tips.html
```
