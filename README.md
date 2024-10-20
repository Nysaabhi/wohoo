<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nysa Post Feed</title>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-color: #1877f2;
            --secondary-color: #f1c40f;
            --text-color: #1c1e21;
            --text-secondary: #65676b;
            --border-color: #e0e0e0;
            --background-color: #f0f2f5;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Roboto', sans-serif;
            background-color: var(--background-color);
            color: var(--text-color);
            line-height: 1.6;
        }

        .container {
            max-width: 600px;
            margin: 20px auto;
            padding: 0 15px;
        }

        .post {
            background-color: white;
            border-radius: 8px;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
            overflow: hidden;
            transition: box-shadow 0.3s ease;
        }

        .post:hover {
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        .post-header {
            display: flex;
            align-items: center;
            padding: 12px 16px;
            border-bottom: 1px solid var(--border-color);
        }

        .post-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            object-fit: cover;
        }

        .post-info {
            margin-left: 12px;
            flex-grow: 1;
        }

        .post-author {
            font-weight: 600;
            font-size: 14px;
        }

        .post-date {
            font-size: 12px;
            color: var(--text-secondary);
        }

        .follow-btn {
            background-color: var(--secondary-color);
            color: white;
            border: none;
            padding: 6px 12px;
            border-radius: 20px;
            font-weight: 500;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        .follow-btn:hover {
            background-color: #e6b80d;
        }

        .follow-btn.following {
            background-color: var(--primary-color);
        }

        .post-image {
            width: 100%;
            max-height: 300px;
            object-fit: cover;
        }

        .post-content {
            padding: 16px;
        }

        .post-title {
            font-size: 18px;
            font-weight: 600;
            margin-bottom: 8px;
        }

        .post-text {
            font-size: 14px;
            color: var(--text-secondary);
        }

        .post-actions {
            display: flex;
            justify-content: space-around;
            padding: 12px 16px;
            border-top: 1px solid var(--border-color);
            background-color: #f8f9fa;
        }

        .action-btn {
            display: flex;
            align-items: center;
            background: none;
            border: none;
            color: var(--text-secondary);
            font-size: 14px;
            cursor: pointer;
            transition: color 0.3s ease;
        }

        .action-btn:hover {
            color: var(--primary-color);
        }

        .action-btn i {
            margin-right: 4px;
        }

        @media (max-width: 480px) {
            .container {
                padding: 0 10px;
            }

            .post-header {
                padding: 10px 12px;
            }

            .post-avatar {
                width: 32px;
                height: 32px;
            }

            .post-author {
                font-size: 13px;
            }

            .post-date {
                font-size: 11px;
            }

            .follow-btn {
                padding: 4px 10px;
                font-size: 12px;
            }

            .post-content {
                padding: 12px;
            }

            .post-title {
                font-size: 16px;
            }

            .post-text {
                font-size: 13px;
            }

            .post-actions {
                padding: 10px 12px;
            }

            .action-btn {
                font-size: 12px;
            }
        }
    </style>
</head>
<body>
    <div class="container" id="post-container"></div>

    <script>
        const posts = [
            {
                id: 1,
                author: "Nysa Post",
                avatar: "https://cryptoexchangereviews.com/wp-content/uploads/2021/04/coinbase-icon2.png",
                date: "Oct 14 • 5 min read",
                image: "https://i.postimg.cc/c19T41qj/Greek-and-Black-Vivid-Bold-Blocks-Electronics-and-Appliances-Banner.png",
                title: "Binance Square New Task Center",
                content: "This is a general announcement. Products and services referred to here may not be available in your region. Stay tuned for more updates on our exciting new features!",
                likes: 6200,
                comments: 3000000,
                shares: 1100,
                retweets: 299,
                replies: 3000
            },
            {
                id: 2,
                author: "Crypto News",
                avatar: "https://cryptoexchangereviews.com/wp-content/uploads/2021/04/coinbase-icon2.png",
                date: "Oct 15 • 3 min read",
                image: "https://i.postimg.cc/C5L74DfM/AD.png",
                title: "Bitcoin Surges to New All-Time High",
                content: "Bitcoin has reached a new all-time high, surpassing previous records. Experts weigh in on the factors driving this surge and what it means for the future of cryptocurrency.",
                likes: 8500,
                comments: 2500000,
                shares: 1500,
                retweets: 450,
                replies: 2200
            },
            {
                id: 3,
                author: "Tech Insider",
                avatar: "https://cryptoexchangereviews.com/wp-content/uploads/2021/04/coinbase-icon2.png",
                date: "Oct 16 • 7 min read",
                image: "https://i.postimg.cc/6q3gYT7K/Black-and-White-Typographic-and-Modern-Phone-Brand-Facebook-Ad.png",
                title: "The Rise of Decentralized Finance (DeFi)",
                content: "Decentralized Finance is reshaping the financial landscape. Learn about the latest trends, opportunities, and challenges in the world of DeFi.",
                likes: 5800,
                comments: 1800000,
                shares: 980,
                retweets: 275,
                replies: 1900
            }
        ];

        function formatNumber(num) {
            if (num >= 1000000) return `${(num / 1000000).toFixed(1)}M`;
            if (num >= 1000) return `${(num / 1000).toFixed(1)}K`;
            return num.toString();
        }

        function createPostElement(post) {
            const postElement = document.createElement('div');
            postElement.className = 'post';
            postElement.innerHTML = `
                <div class="post-header">
                    <img src="${post.avatar}" alt="${post.author}" class="post-avatar">
                    <div class="post-info">
                        <div class="post-author">${post.author}</div>
                        <div class="post-date">${post.date}</div>
                    </div>
                    <button class="follow-btn">Follow</button>
                </div>
                <img src="${post.image}" alt="${post.title}" class="post-image">
                <div class="post-content">
                    <h2 class="post-title">${post.title}</h2>
                    <p class="post-text">${post.content}</p>
                </div>
                <div class="post-actions">
                    <button class="action-btn like-btn" data-count="${post.likes}">
                        <i class="fas fa-heart"></i> <span>${formatNumber(post.likes)}</span>
                    </button>
                    <button class="action-btn">
                        <i class="fas fa-comment"></i> ${formatNumber(post.comments)}
                    </button>
                    <button class="action-btn">
                        <i class="fas fa-share"></i> ${formatNumber(post.shares)}
                    </button>
                    <button class="action-btn">
                        <i class="fas fa-retweet"></i> ${formatNumber(post.retweets)}
                    </button>
                    <button class="action-btn">
                        <i class="fas fa-reply"></i> ${formatNumber(post.replies)}
                    </button>
                </div>
            `;

            const followBtn = postElement.querySelector('.follow-btn');
            followBtn.addEventListener('click', () => {
                followBtn.classList.toggle('following');
                followBtn.textContent = followBtn.classList.contains('following') ? 'Following' : 'Follow';
            });

            const likeBtn = postElement.querySelector('.like-btn');
            likeBtn.addEventListener('click', () => {
                let count = parseInt(likeBtn.dataset.count);
                if (likeBtn.classList.contains('liked')) {
                    count--;
                    likeBtn.classList.remove('liked');
                } else {
                    count++;
                    likeBtn.classList.add('liked');
                }
                likeBtn.dataset.count = count;
                likeBtn.querySelector('span').textContent = formatNumber(count);
            });

            return postElement;
        }

        function renderPosts() {
            const container = document.getElementById('post-container');
            posts.forEach(post => {
                const postElement = createPostElement(post);
                container.appendChild(postElement);
            });
        }

        document.addEventListener('DOMContentLoaded', renderPosts);
    </script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/js/all.min.js"></script>
</body>
</html>
