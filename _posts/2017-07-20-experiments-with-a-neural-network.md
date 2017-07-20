---
title: Experiments with a Neural Network
layout: post
---

What happens when you feed all of Radiohead’s lyrics to a neural network and tell it to write you a song?

Neural networks are a branch of artificial intelligence that aims to create machines that learn from being fed data. The idea is to mimic the human brain to enable everything from generating captions for images to composing music.

I decided to experiment with the [torch-rnn](https://github.com/jcjohnson/torch-rnn) project to see how easily I could get it to generate song lyrics.

There are two main components to this project:
1. Scraping lyrics to feed to the network.
2. Training the network and generating output.

## Scraping Source Material

For getting lyrics, I used the amusingly named `tswift` Python library ([https://pypi.python.org/pypi/tswift](https://pypi.python.org/pypi/tswift)). This library provides an API for accessing the [MetroLyrics] database. (It also provides a CLI command for printing a random Taylor Swift lyric, but that wasn’t as useful.)

I created a text file to store the lyrics and ran the following script to scrape them using `tswift`.

{% highlight python %}
import tswift

# Set the artist to use
artist = tswift.Artist('radiohead')

# Start with a clean text file
with open('./lyrics.txt', 'w') as l:
	l.write('')

# Iterate over all of the artist's songs
for song in artist:
	try:
		lyrics = song.lyrics

	# Write the lyrics to the text file
	with open('./lyrics.txt', 'a') as l:
		l.write(lyrics + '\n')
{% endhighlight %}

## Generating the Output

Once I had all the lyrics ready to go in a single text file, I needed to install `torch-rnn` and all its dependencies. Jeff Thompson has a wonderful guide on getting it set up on macOS on his website ([https://www.jeffreythompson.org/blog/2016/03/25/torch-rnn-mac-install](https://www.jeffreythompson.org/blog/2016/03/25/torch-rnn-mac-install)).

To actually get output from `torch-rnn`, I prepped the data using the included Python script, trained the network on my lyrics text file, and took a sample. Shell commands for these three operations are included below, with some customizations from the default that yielded better results for my output.

Preparing the data:
`python3 scripts/preprocess.py --input_txt data/lyrics.txt --output_h5 data/lyrics.h5 --output_json data/lyrics.json`

Training the network:
`th train.lua -gpu -1 -input_h5 data/lyrics.h5 -input_json data/lyrics.json -batch_size 1 -seq_length 35`

Generating the output:
`th sample.lua -gpu -1 -checkpoint cv/checkpoint_179250.t7 -length 750 > data/lyrics_output.txt -sample 1`

## The Results

It took several tries generating output, but I eventually got some short snippets that were not too bad. My favorite is the continuous stretch from one output included below.

> You float leave
>
> Worms in the gal in  
> The dark to me again if I squeal to the chimbe  
> All the good times  
> All the light pour
>
> Mings a song?
>
> Woh's ending in  
> The last she new it ever haital  
> Your way again, he want

If you ignore the nonsensicality and made-up words, it’s… not bad. There’s definitely a cadence to it that feels pretty poetic.

## Closing Thoughts

There are many ways that things could be improved, both with scraping the lyrics and with the neural network. But even with minimal effort on my end, I was impressed with what `torch-rnn` could do.
