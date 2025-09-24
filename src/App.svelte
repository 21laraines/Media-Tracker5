<script>

  //---------------------------------------------------------------
  //---------------------------------------------------------------
  // For "Goal Progress" on the left Profile side of the screen
  let readingTotal = 0;
  let musicTotal = 0;
  let goalProgressMessage =[];
  let readingChecked = false;
  let musicChecked = false;


  //---------------------------------------------------------------
  //---------------------------------------------------------------
  // For "Today's Goals"
  let newGoalText = '';
  let customGoals = [];

  function addGoal(event) {
    event.preventDefault();
    alert("A new goal has been added!");
    const trimmedGoal = newGoalText.trim();
    if (trimmedGoal !== '') {
      customGoals = [...customGoals, trimmedGoal];
      newGoalText = '';
    }
  }

  function dailyGoals(event) {
    event.preventDefault(); 
    alert("Your goals have been submitted for today!");
    goalProgressMessage = [];
    if (readingChecked) {
      readingTotal += 10;
      goalProgressMessage.push(`You've read ${readingTotal} pages!`);
      readingChecked = false; 
    }

    if (musicChecked) {
      musicTotal += 1;
      goalProgressMessage.push(`You've listened to ${musicTotal} new song${musicTotal > 1 ? 's' : ''}!`);
      musicChecked = false;
    }
    
  }
  
  //---------------------------------------------------------------
  //---------------------------------------------------------------
  // For "Completed Media"
  let completedBooks =[];
  let completedMusic =[];
  let completedVM = [];

  //---------------------------------------------------------------
  //---------------------------------------------------------------
  // For "Current Books"
  let showBookForm = false;
  let isEditingBook = false;
  let isBookCompleted = false;
  let editingBookIndex = null;
  let title = '';
  let author = '';
  let currentPage = '';
  let coverFile;
  let coverPreviewUrl = '';
  let books = [{title: "Tomorrow Tomorrow and Tomorrow", author: "Gabrielle Zevin", currentPage: "254", coverPreviewUrl:"Tomorrow x3.jpg"}];
  let review = '';
  let rating = '1';

  function handleFileChange(event) {
    const file = event.target.files[0];
    if (file) {
      coverFile = file;
      coverPreviewUrl = URL.createObjectURL(file); 
    }
  }

  function submitBook() {
    const newBook = {
      title,
      author,
      currentPage,
      coverPreviewUrl, 
      review, 
      rating
    };

    if (isEditingBook && editingBookIndex !== null) {
      if(isBookCompleted){
        completedBooks = [...completedBooks, newBook];
        books.splice(editingBookIndex, 1);
      }
      else{
        books[editingBookIndex] = newBook;
      }
      books = [...books]; 
    } else {
      if(isBookCompleted){
        completedBooks = [...completedBooks, newBook];
      }
      else{
      books = [...books, newBook];
      }
    }

    coverFile = null;
    coverPreviewUrl = '';
    title = '';
    author = '';
    currentPage = '';
    isEditingBook = false;
    editingBookIndex = null;
    showBookForm = false;
    isBookCompleted = false;
    review = '';
    rating = '1';
  }
  function editBook(index) {
  const book = books[index];
  title = book.title;
  author = book.author;
  currentPage = book.currentPage;
  coverPreviewUrl = book.coverPreviewUrl;
  isEditingBook = true;
  editingBookIndex = index;
  isBookCompleted = false;
  showBookForm = true;
}
  //---------------------------------------------------------------
  //---------------------------------------------------------------

  //For "Current Music"
   let albums = [
    {
      albumTitle: "The Crux Deluxe", artist: "djo", favSongs: "Away, Golden Egg",albumCoverPreviewUrl: "The Crux.jpg"}
  ];

  let showMusicForm = false;
  let albumTitle = '';
  let artist = '';
  let favSongs = '';
  let albumCoverFile;
  let albumCoverPreviewUrl = '';
  let musicReview = "";
  let musicRating = 1;
  let isEditingMusic = false;
  let editingMusicIndex = null;
  let isMusicCompleted = false;

  function handleAlbumCoverChange(event) {
    const file = event.target.files[0];
    if (file) {
      albumCoverFile = file;
      albumCoverPreviewUrl = URL.createObjectURL(file);
    }
  }

  function submitMusic() {
    const newAlbum = {
      albumTitle,
      artist,
      favSongs,
      albumCoverPreviewUrl,
      musicReview,
      musicRating
    };

    if (isEditingMusic && editingMusicIndex !== null) {
      if (isMusicCompleted) {
        completedMusic = [...completedMusic, newAlbum];
        albums.splice(editingMusicIndex, 1);
      } 
    else {
      albums[editingMusicIndex] = newAlbum;
    }
    albums = [...albums];
    } 
      else {
      if (isMusicCompleted) {
        completedMusic = [...completedMusic, newAlbum];
      } 
      else {
        albums = [...albums, newAlbum];
      }
    }

    albumCoverFile = null;
    albumCoverPreviewUrl = '';
    albumTitle = '';
    artist = '';
    favSongs = '';
    showMusicForm = false;
    isEditingMusic = false;
    editingMusicIndex = null;
    isMusicCompleted = false;
    musicReview = '';
    musicRating = 1;
  }

  function editMusic(index) {
    const album = albums[index];
    albumTitle = album.albumTitle;
    artist = album.artist;
    favSongs = album.favSongs;
    albumCoverPreviewUrl = album.albumCoverPreviewUrl;
    musicReview = album.musicReview || '';
    musicRating = album.musicRating || '1';
    isEditingMusic = true;
    editingMusicIndex = index;
    isMusicCompleted = false;
    showMusicForm = true;
  }

  //---------------------------------------------------------------
  //---------------------------------------------------------------

  //For "Current Visual Media"
  let visualMedia = [{VMTitle: "Gilmore Girls", notes: "Season 1 and 2 are the best", VMCoverPreviewUrl: "Gilmore Girls.jpg"}];
  let showVMForm = false;
  let VMTitle = '';
  let notes = '';
  let VMCoverFile;
  let VMCoverPreviewUrl = '';
  let VMRating = 1;
  let VMReview = "";
  let isEditingVM = false;
  let editingVMIndex = null;
  let isVMCompleted = false;

  function handleVMCoverChange(event) {
    const file = event.target.files[0];
    if (file) {
      VMCoverFile = file;
      VMCoverPreviewUrl = URL.createObjectURL(file);
    }
  }

  function submitVM() {
    const newVM = {
      VMTitle,
      genres: [...selectedGenres],
      notes,
      VMRating,
      VMReview,
      VMCoverPreviewUrl
    };

    if (isEditingVM && editingVMIndex !== null) {
      if (isVMCompleted) {
        completedVM = [...completedVM, newVM];
        visualMedia.splice(editingVMIndex, 1);
    } 
      else {
      visualMedia[editingVMIndex] = newVM;
      }
    visualMedia = [...visualMedia];
    } 
    else {
      if (isVMCompleted) {
        completedVM = [...completedVM, newVM];
      } 
      else {
        visualMedia = [...visualMedia, newVM];
      }
    }
    VMCoverFile = null;
    VMCoverPreviewUrl = '';
    VMTitle = '';
    notes = '';
    VMReview = '';
    VMRating = 1;
    selectedGenres = [];
    isEditingVM = false;
    editingVMIndex = null;
    isVMCompleted = false;
    showVMForm = false;
  }

  let genreOptions = [
  "Drama",
  "Comedy",
  "Action",
  "Romance",
  "Thriller",
  "Sci-Fi",
  "Fantasy",
  "Documentary"
  ];

  let selectedGenres = [];
  function toggleGenre(genre) {
  if (selectedGenres.includes(genre)) {
    selectedGenres = selectedGenres.filter(g => g !== genre);
  } else {
    selectedGenres = [...selectedGenres, genre];
  }
  }

  function editVM(index){
    const item = visualMedia[index];
    VMTitle = item.VMTitle;
    notes = item.notes;
    VMCoverPreviewUrl = item.VMCoverPreviewUrl;
    selectedGenres = [...item.genres || []];
    VMReview = item.VMReview || "";
    VMRating = item.VMRating || 1;
    isEditingVM = true;
    editingVMIndex = index;
    isVMCompleted = false;
    showVMForm = true;
  }
  
</script>

<main>

  <!--Profile side (Left side)-->
  <div class="container">
    <div class="Profile-side">
      <h1 class="webpageName">Media Tracker</h1>
      <section>
        <img src="Default Profile Picture.jpg" alt="Default Profile Picture" class="profileImage">
      </section>
      <h2>Welcome back, Jane!</h2>
      <p>September 24, 2025</p>
      <p>10 days beginning, 3 days active</p>
      <div class="GoalProgress">
        <h2>Goal Progress</h2>
        {#if goalProgressMessage.length > 0}
          <ul>
            {#each goalProgressMessage as message}
              <li>{message}</li>
            {/each}
          </ul>
          {/if}
      </div>
      <div class="note">
        <p><em>*This is a prototype application. Changes made will not be saved after you refresh or leave the page.*</em></p>
      </div>
    </div>
  </div>

  <!------------------------------------------------------------------------------------------>
  <!------------------------------------------------------------------------------------------>
  <!--Media Info (Right side)-->
  <div class="container">
    <div class="Media-Side">
      <!--The daily goals section-->
      <section class="Goals-section">
      <h1>Today's Goals</h1>
        <form id="goals" on:submit|preventDefault={dailyGoals}>
          <!-- Default Goals -->
          <input type="checkbox" id="readingGoal" name="goal" value="numOfPages" bind:checked={readingChecked}>
          <label for="readingGoal">I have read 10 pages.</label>

          <input type="checkbox" id="musicGoal" name="goal" value="newGenre" bind:checked={musicChecked}>
          <label for="musicGoal">I have listened to a new song.</label>

          <!-- Added Goals -->
          {#each customGoals as goal, index}
            <input type="checkbox" id={"customGoal" + index} name="goal" value={goal}>
            <label for={"customGoal" + index}>{goal}</label>
          {/each}

          <button type="submit" id="submitgoals">Submit</button>

          <div>
            <input type="text" id="newGoal" placeholder="Add a new goal..." bind:value={newGoalText} />
            <button on:click={addGoal}>Add Goal</button>
          </div>
        </form>
      </section>

      <!------------------------------------------------------------------------------------------>
      <!------------------------------------------------------------------------------------------>
      <!--This section lists what the user is currently reading-->
      <section class="Books">
        <h1>Current Books</h1>
          <button on:click={() => showBookForm = !showBookForm}>
            {showBookForm ? 'Cancel' : 'Add New Book'}
          </button>
            {#if showBookForm}
              <form on:submit|preventDefault={submitBook} class="book-form">
                <label>
                  Book Cover Image:
                  <input type="file" accept="image/*" on:change={handleFileChange} />
                </label>

                {#if coverPreviewUrl}
                  <img src={coverPreviewUrl} alt="Book Cover" class="book-cover-preview" />
                {/if}
                <label>
                  Title:
                  <input type="text" bind:value={title} required />
                </label>

                <label>
                  Author:
                  <input type="text" bind:value={author} required />
                </label>

                <label>
                  Current Page:
                  <input type="number" bind:value={currentPage} required min="1" />
                </label>
                <label>
                  Rating:
                  <select bind:value={rating}>
                    <option value="1">1 out of 5</option>
                    <option value="2">2 out of 5</option>
                    <option value="3">3 out of 5</option>
                    <option value="4">4 out of 5</option>
                    <option value="5">5 out of 5</option>
                  </select>
                </label>
                <label>
                  Review:
                  <textarea bind:value={review} rows="4" placeholder="Write your thoughts..."></textarea>
                </label>
                <label>
                  <input type="checkbox" bind:checked={isBookCompleted}>
                  Mark as completed
                </label>
                <button type="submit">Save Book</button>
              </form>
            {/if}
            <!--Edit Books-->
            {#if books.length > 0}
              <ul class="book-list">
                {#each books as book, index}
                  <li class="book-item">
                    <h3>{book.title}</h3>
                      <p><strong>Author:</strong> {book.author}</p>
                      <p><strong>Pages:</strong> {book.currentPage}</p>
                      {#if book.coverPreviewUrl}
                        <img src={book.coverPreviewUrl} alt="Book Cover" class="book-cover-display" />
                      {/if}
                      <button on:click={() => editBook(index)}>Edit</button>
                  </li>
                {/each}
              </ul>
            {/if}
      </section>

      <!------------------------------------------------------------------------------------------>
      <!------------------------------------------------------------------------------------------>
      <!--This section is for things the user is currently listening to-->
      <section class="Music">
        <h1>Current Music</h1>
        <button on:click={() => showMusicForm = !showMusicForm}>
          {showMusicForm ? 'Cancel' : 'Add New Music'}
        </button>

        {#if showMusicForm}
          <form on:submit|preventDefault={submitMusic} class="music-form">
            <label>
              Album Image:
              <input type="file" accept="image/*" on:change={handleAlbumCoverChange} />
            </label>
            {#if albumCoverPreviewUrl}
              <img src={albumCoverPreviewUrl} alt="Album Cover" class="album-cover-preview" />
            {/if}
            <label>
              Title:
              <input type="text" bind:value={albumTitle} required />
            </label>
            <label>
              Artist(s):
              <input type="text" bind:value={artist} required />
            </label>
            <label>
              Favorite Songs:
              <input type="text" bind:value={favSongs} required />
            </label>
            <label>
              Rating:
              <select bind:value={musicRating}>
                <option value="1">1 / 5</option>
                <option value="2">2 / 5</option>
                <option value="3">3 / 5</option>
                <option value="4">4 / 5</option>
                <option value="5">5 / 5</option>
              </select>
            </label>
            <label>
              Review:
              <textarea bind:value={musicReview} rows="4" placeholder="Your thoughts..."></textarea>
            </label>
            <label>
              <input type="checkbox" bind:checked={isMusicCompleted} />
              Mark as Completed
            </label>
            <button type="submit">{isEditingMusic ? 'Save Changes' : 'Save Album'}</button>
          </form>
        {/if}

        <!--Edit music-->
        {#if albums.length > 0}
          <ul class="album-list">
            {#each albums as album, index}
              <li class="album-item">
                <h3>{album.albumTitle}</h3>
                <p><strong>Artist:</strong> {album.artist}</p>
                <p><strong>Favorite Songs:</strong> {album.favSongs}</p>
                {#if album.albumCoverPreviewUrl}
                  <img src={album.albumCoverPreviewUrl} alt="Album Cover" class="album-cover-display" />
                {/if}
                <button on:click={() => editMusic(index)}>Edit</button>
              </li>
            {/each}
          </ul>
        {/if}
      </section>

      <!------------------------------------------------------------------------------------------>
      <!------------------------------------------------------------------------------------------>
      <!--This section is for things the user is currently watching-->
      <section class="Films">
        <h1>Current Visual Media</h1>
        <button on:click={() => showVMForm = !showVMForm}>
          {showVMForm ? 'Cancel' : 'Add New'}
        </button>

        {#if showVMForm}
          <form on:submit|preventDefault={submitVM} class="VM-form">
              <label>
                Show/Movie Image:
                <input type="file" accept="image/*" on:change={handleVMCoverChange} />
              </label>

              {#if VMCoverPreviewUrl}
                <img src={VMCoverPreviewUrl} alt="Cover" class="VM-cover-preview" />
              {/if}
              <label>
                Title:
                <input type="text" bind:value={VMTitle} required />
              </label>
              <label>
                Notes:
                <textarea bind:value={notes} rows="5" cols="10"></textarea>
              </label>
              <p>Genres:</p>
              <div class="genre-checkboxes">
                {#each genreOptions as genre}
                  <label>
                    <input type="checkbox" value={genre} on:change={() => toggleGenre(genre)} checked={selectedGenres.includes(genre)} />
                    {genre}
                  </label>
                {/each}
              </div>
              <label>
                Rating:
                <select bind:value={VMRating}>
                  <option value="1">1 / 5</option>
                  <option value="2">2 / 5</option>
                  <option value="3">3 / 5</option>
                  <option value="4">4 / 5</option>
                  <option value="5">5 / 5</option>
                </select>
              </label>
              <label>
                Review:
                <textarea bind:value={VMReview} rows="4" placeholder="Your thoughts..."></textarea>
              </label>
              <label>
                <input type="checkbox" bind:checked={isVMCompleted} />
                Mark as Completed
              </label>
              <button type="submit">{isEditingVM ? 'Save Changes' : 'Save Visual Media'}</button>
          </form>
        {/if}
        

        <!-- Shows the saved visual media -->
        {#if visualMedia.length > 0}
          <ul class="book-list">
            {#each visualMedia as vm, index}
              <li class="book-item">
                <h3>{vm.VMTitle}</h3>
                {#if vm.genres?.length}
                  <p><strong>Genre(s):</strong> {vm.genres.join(', ')}</p>
                {/if}
                <p><strong>Notes:</strong> {vm.notes}</p>
                {#if vm.VMCoverPreviewUrl}
                  <img src={vm.VMCoverPreviewUrl} alt="Visual Media Cover" class="book-cover-display" />
                {/if}
                <button on:click={() => editVM(index)}>Edit</button>
              </li>
            {/each}
          </ul>
        {/if}
      </section>

      <!------------------------------------------------------------------------------------------>
      <!------------------------------------------------------------------------------------------>
      <!--This section will hold all of the completed media-->
      <section class="Finished-Media">
        <h1>Finished Media</h1>
<!------------------------Finished Books---------------------->
            {#if completedBooks.length > 0}
              <ul class="book-list">
                {#each completedBooks as book}
                  <li class="book-item">
                    <h3>{book.title}</h3>
                    <p><strong>Author:</strong> {book.author}</p>
                    <p><strong>Pages:</strong> {book.currentPage}</p>
                    {#if book.rating}
                      <p><strong>Rating:</strong> {book.rating} / 5</p>
                    {/if}
                    {#if book.review}
                      <p><strong>Review:</strong> {book.review}</p>
                    {/if}
                    {#if book.coverPreviewUrl}
                      <img src={book.coverPreviewUrl} alt="Book Cover" class="book-cover-display" />
                    {/if}
                  </li>
                {/each}
              </ul>
            {/if}
<!------------------------Finished Music---------------------->
            {#if completedMusic.length > 0}
              <ul class="album-list">
                {#each completedMusic as album}
                  <li class="album-item">
                    <h3>{album.albumTitle}</h3>
                    <p><strong>Artist:</strong> {album.artist}</p>
                    <p><strong>Favorite Songs:</strong> {album.favSongs}</p>
                    {#if album.musicRating}
                      <p><strong>Rating:</strong> {album.musicRatingating} / 5</p>
                    {/if}
                    {#if album.musicReview}
                      <p><strong>Review:</strong> {album.musicReview}</p>
                    {/if}
                    {#if album.albumCoverPreviewUrl}
                      <img src={album.albumCoverPreviewUrl} alt="Album Cover" class="album-cover-display" />
                    {/if}
                  </li>
                {/each}
              </ul>
            {/if}
<!------------------------Finished Visual Media---------------------->
            {#if completedVM.length > 0}
              <ul class="vm-list">
                {#each completedVM as vm}
                  <li class="vm-item">
                    <h3>{vm.VMTitle}</h3>
                    <p><strong>Genere(s):</strong> {vm.genre.join(', ')}</p>
                    {#if vm.VMRating}
                      <p><strong>Rating:</strong> {vm.VMRating} / 5</p>
                    {/if}
                    {#if vm.VMReview}
                      <p><strong>Review:</strong> {vm.VMReview}</p>
                    {/if}
                    {#if vm.VMCoverPreviewUrl}
                      <img src={vm.VMCoverPreviewUrl} alt="Cover" class="vm-cover-display" />
                    {/if}
                  </li>
                {/each}
              </ul>
            {/if}

            {#if completedBooks.length === 0 && completedMusic.length === 0 && completedVM.length === 0}
              <p>No finished media yet.</p>
            {/if}
      </section>
    </div>
  </div>
  
</main>


<style>
  

</style>
