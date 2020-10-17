# Watching movies: How to use a DVD with a Synology NAS
[Synology has a bad reputation for it's Video Station](https://community.synology.com/enu/forum/17/post/93830). With this post I am going to explore how to play those copyied .ISO files stored on my Synology on my Samsung TV.

Resources: https://www.easefab.com/tutorial/play-iso-files-on-samsung-tv.html


# Getting the ISO from the DVD
Assuming, you have a single SCSI drive (which is the most common type):

    # get the required block and volume size first
    isoinfo -d -i /dev/sr0 | grep -i -E 'block size|volume size'
    # copy the media content into a single ISO file, using the block size as 'bs' and the volume size as 'count', like in the example below
    sudo dd if=/dev/sr0 of=movie.iso bs=2048 count=2811981

After a moment, this should get you the ISO file.
