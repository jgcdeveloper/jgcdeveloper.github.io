---
layout: post
title: Bloc Jams - Part A
thumbnail-path: "/img/BJams_Basic.jpg"
short-description: Bloc Jams a learning project for Bloc.io. It re-creates the functionality of a basic music player online, using HTML5 music and Buzz Music API.

---

{:.center}
![]({{ site.baseurl }}/img/BJams_Basic.jpg)

## Explanation

Bloc Jams is our first learning project for the online bootcamp Bloc.io. At the time of writing this study,it has been used to teach students (in this case, myself) the basics of HTML, CSS, Javascript, and jQuery. We use techniques such as building a basic grid system with CSS, DOM selection using both vanilla JS and jQuery, and how to class selections when building the HTML. Much of it is guided, although there are several assignments where we have freedom to write our own code. We are also using Bloc Jams to teach us the basics of GIT for use as a version control system, although this is in its simpilest form as we don't have to deal with multiple people pushing and pulling from the same repository.

Case Study Written: October 3rd, 2016

GitHub Link: [Bloc-Jams GitHub Repository](https://github.com/jgcdeveloper/bloc-jams)

## Problem

Bloc Jams is seperated into checkpoints, where we are guided through creating the website, and assignments where we get to take responsiblity for coding ourselves a little bit more. I look forward to that the most, as I like a challenge.

One such example, is where we were given a task to refactor code to make it more DRY. In our project we have a player bar, which has a previous and a next button. We were guided to basically write two different functions, which had a lot of repetition. We were asked to refactor it to one function call, passing in which button was clicked.

Our functions as they exsisted prior to refactoring:


{% highlight javascript %}

var nextSong = function(){

    //Do nothing if no song is playing
    if(currentSongFromAlbum !== null){

        //Get the current track index..
        var currentSongIndex = trackIndex(currentAlbum, currentSongFromAlbum);

        //Increment the current index
        currentSongIndex++;

        //Is the incremented currentSongIndex greater then the length of songs, if so wrap currentSongIndex back to zero
        currentSongIndex >= currentAlbum.songs.length ? currentSongIndex = 0 : currentSongIndex;

        //Update our currentSong and currentlyPlayingSongNumber variables
        currentSongFromAlbum = currentAlbum.songs[currentSongIndex];
        currentlyPlayingSongNumber = (currentSongIndex + 1).toString();  

        //Update the NEW currentlyPlayingSong icon to a pause button
        var $newSong = $('.song-item-number[data-song-number="' + currentlyPlayingSongNumber + '"]');
        $newSong.html(pauseButtonTemplate);

        //Set the PREVIOUS song back to a number from the icon  
        var prevSong = currentlyPlayingSongNumber == '1' ? currentAlbum.songs.length : currentSongIndex;
        $('.song-item-number[data-song-number="' + prevSong + '"]').html(prevSong);

        //Update player bar
        updatePlayerBarSong();
    }
}

var previousSong = function(){

    //Do nothing if no song is playing
    if(currentSongFromAlbum !== null){

        //Get the current track index..
        var currentSongIndex = trackIndex(currentAlbum, currentSongFromAlbum);

        //Increment the current index
        currentSongIndex--;

        //Roll songIndex back to song length (-1 for the index from length) if at 0, else keep the number
        currentSongIndex < 0 ? currentSongIndex = currentAlbum.songs.length - 1 : currentSongIndex;

        //Update our currentSong and currentlyPlayingSongNumber variables
        currentSongFromAlbum = currentAlbum.songs[currentSongIndex];
        currentlyPlayingSongNumber = (currentSongIndex + 1).toString();  

        //Update the NEW currentlyPlayingSong icon to a pause button
        var $newSong = $('.song-item-number[data-song-number="' + currentlyPlayingSongNumber + '"]');
        $newSong.html(pauseButtonTemplate);

        //Set the PREVIOUS song back to a number from the icon  
        var prevSong = currentlyPlayingSongNumber == '5' ? 1 : currentSongIndex + 2;
        $('.song-item-number[data-song-number="' + prevSong + '"]').html(prevSong);

        //Update player bar
        updatePlayerBarSong();

    }
};

{% endhighlight %}

Then we set up a two event listeners to see when the previous or next buttons were clicked. That is coded here:

{% highlight javascript %}

$previousButton.click(previousSong);
$nextButton.click(nextSong);

{% endhighlight %}

## Solution

One solution here would be to perhaps add another class (or rewrite next and previous buttons into a child class) so that we could place a single event handler on them for both buttons. Then, when either button was clicked, we could declare a variable inside the new function using event to find out and match which element was clicked, and write our conditionals based upon that new variable.

That might reduce our code a bit more then my solution, to be honest, but the way I did it (detailed below) I think keeps things more readible. It also is less likely to break things because I am not writing any new classes into our HTML or making any other major changes where bugs could be introducted. That might put D.R.Y. into conflict with K.I.S.S. and IIAB/DFI (If it ain't broke, don't fix it), but I think my solution respects all of these.


---

My solution was to instead keep the two functions nextSong() and previousSong(), but actually write the code that does the work into a third function, called buttonsPrevNext(n); Inside the nextSong() and previousSong(), we simply execute a call to the buttonsPrevNext() function passing a variable indicating which button was pressed. In this case, 0 or 1.

In the code, that looks like

{% highlight javascript %}

var nextSong = function(){ buttonsNextPrev(0); };
var previousSong = function(){ buttonsNextPrev(1); };

var buttonsNextPrev = function(selector){

    //Do nothing if no song is playing
    if(currentSongFromAlbum !== null){

        //Get the current track index..
        var currentSongIndex = trackIndex(currentAlbum, currentSongFromAlbum);

        //Updates our song index to the new or previous song. Loops if we are at the end or beginning.
        if(selector === 0){
            currentSongIndex++;
            currentSongIndex >= currentAlbum.songs.length ? currentSongIndex = 0 : currentSongIndex;
        } else {
            currentSongIndex--;
            currentSongIndex < 0 ? currentSongIndex = currentAlbum.songs.length - 1 : currentSongIndex;
        }

        //set new song
        setSong(currentSongIndex + 1);

        //Update the NEW currentlyPlayingSong icon to a pause button
        getSongNumberCell(currentlyPlayingSongNumber).html(pauseButtonTemplate);

        //Our bars have a play icon next to them when song is palying. This code needs to replace that play on the old song with the original number
        if(selector === 0){
            var prevSong = ( currentlyPlayingSongNumber == 1 ? currentAlbum.songs.length : currentSongIndex );
            getSongNumberCell(prevSong).html(prevSong);
        } else {
            var prevSong = ( currentlyPlayingSongNumber == 5 ? 1 : currentlyPlayingSongNumber + 1 );
            getSongNumberCell(prevSong).html(prevSong);    
        }

        //Update player bar
        updatePlayerBarSong();

    };

};

{% endhighlight %}

## Results

In the end, you can see that indeed it does make the code more D.R.Y. One other thing I like about this solution is it keeps the buttonsNextPrev() function from needing to do too many things. In my reading, I often read that many senior programmers believe you should endeavor to keep each function to have only one specifc task. This not only makes the code easier to follow, but also makes it easier to add changes to the original code.

In this case, say I wanted to add other functionality (for example, a transition) when we click on our previous or next buttons. If I had removed the nextButton and prevButton completely and put everything into one combined function, I would need to add on or change that function, possibly adding bugs. However, with the nextSong and previousSong functions still intact, I could simply write the new transition into a seperate function and call both when the button is clicked (remember we have nextSong and previousSong as the callbacks for our click event listener).

{% highlight javascript %}

var nextSong = function(){
  buttonsNextPrev(0);
  myNewTransition();
};

var previousSong = function(){
  buttonsNextPrev(1);
  myNewTransition();
};

{% endhighlight %}

Now, even looking at this a week or so after completion, I realize that there is additional opportunity for refactoring the code by collapsing the two if/else loops into one loop. Why did I not do this originally? I think it is because as a learning developer I am still thinking through things in a step by step manner. As such, before I write any function of more then a few steps, I try to write out how the function works in psuedo code ( in this case study, basically what is commented in // before and each section). When I originally wrote the psued code, I had the order laid out pretty much the same as I have the code laid out now.

Why did I not change it before writing the case study? I didn't change it because I want to highlight the fact that I am learning on a very aggressive scale, and how I do things today are not the same as I might have done yesterday or even last week. It is something to keep in mind when vieweing this case, and any other cases on this website.

I am new to this industry, but hungry to learn, and absorbing information at a rapid pace!!

## Conclusion

Bloc Jams is a learning project. While guided, it introduced us to many basic concepts of front end development with HTML / CSS / JS. I think most important is that it got us as students to start to write actual code, while at the same time helping us to understand why things are coded the way they are.

This represents only the tip of the tip of the iceberg that is this industry, but I have made a great start, and more importantly am hungry to learn more. At six weeks into my career conversion journey, I think that is a good thing!
