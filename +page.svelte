<script lang="ts">
    import Footer from '../../components/Footer.svelte';
    import Header from '../../components/Header.svelte';
    import Item from '../../components/Item.svelte';
    import Paginator from '../../components/Paginator.svelte';

    interface Song{
        title:string;
        album:string;
        artist:string;
    }

    interface Review{
        blurb:string;
        rating:number;
    }

    let songs:Array<Song> = [
        {title:"1", artist:"Porter Robinson", album:"Album"},
        {title:"2 to a World", artist:"Porter Robinson", album:"Album"},
        {title:"3 number", artist:"xaev", album:"berdlycore chapter 2"},
        {title:"4", artist:"Porter Robinson", album:"Album"},
        {title:"5 to a World", artist:"Porter Robinson", album:"Album"},
        {title:"6", artist:"Porter Robinson", album:"Album"},
        {title:"7 to a World", artist:"Porter Robinson", album:"Album"},
        {title:"8", artist:"Porter Robinson", album:"Album"},
        {title:"9 to a World", artist:"Porter Robinson", album:"Album"},
        {title:"10 number", artist:"xaev", album:"berdlycore chapter 2"}
    ];

    let reviews:Array<Review> = [
        {blurb:"Song1", rating:4.5},
        {blurb:"Song2", rating:5},
        {blurb:"We are so back", rating:4.5},
        {blurb:"I could die to this", rating:5},
        {blurb:"This song is so dumb lol", rating:3},
        {blurb:"We are so back", rating:4.5},
        {blurb:"I could die to this", rating:5},
        {blurb:"We are so back", rating:4.5},
        {blurb:"I could die to this", rating:5},
        {blurb:"This song is so dumb lol", rating:3}
    ];
</script>

<Paginator items={songs} elementsPerPage={4}></Paginator>
