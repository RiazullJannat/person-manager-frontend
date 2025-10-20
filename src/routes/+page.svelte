<section
    class="relative min-h-screen bg-gradient-to-tr from-blue-900 via-gray-900 to-black flex items-center justify-center px-6"
>
    <div class="absolute inset-0 bg-opacity-30 bg-black backdrop-blur-sm"></div>

    <div class="relative z-10 text-center max-w-3xl text-white animate-fade-in">
        <h1 class="text-4xl md:text-6xl font-bold leading-tight">
            Welcome to <br />
            <span class="text-blue-400">Person Management Application!</span>
        </h1>

        <p class="mt-6 text-lg text-gray-300">
            Effortlessly manage people, track data and build smarter
            interactions.
        </p>

        <div class="flex justify-center mt-10 space-x-4">
            <!-- All Persons Button -->
            <a
                href="/all-persons"
                target="_blank"
                rel="noopener noreferrer"
                class="flex items-center gap-2 px-6 py-3 bg-blue-600 hover:bg-blue-700 text-white font-semibold rounded-lg shadow-lg transition duration-300"
            >
                <!-- Icon: Users -->
                <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="h-5 w-5"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                >
                    <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M17 20h5v-2a4 4 0 00-4-4h-1m-6 6H2v-2a4 4 0 014-4h1m6 0a4 4 0 100-8 4 4 0 000 8zm6-4a4 4 0 11-8 0 4 4 0 018 0z"
                    />
                </svg>
                View All Persons
            </a>

            <!-- Add New Person Button -->
            <a
                href="/add-person"
                class="flex items-center gap-2 px-6 py-3 bg-transparent border border-gray-400 hover:border-white text-gray-300 hover:text-white rounded-lg transition duration-300"
            >
                <!-- Icon: Plus -->
                <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="h-5 w-5"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                >
                    <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M12 4v16m8-8H4"
                    />
                </svg>
                Add New Person
            </a>
        </div>
    </div>
</section>
