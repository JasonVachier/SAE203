# DOCUMENTATION SAE203

## C'est ici que ce trouve style.css

    @font-face {
        font-family: fontHeader;
        src: url(Prototype.ttf);
    }

    header {    
        display: flex;
        flex-direction: column; 
        align-items: center; 
        justify-content: center; 
        font-family: fontHeader;
        font-size: 30px;
        font-weight: bold;
        background-color: rgba(0, 0, 0, 0.5);
        color: #dfdfdfef;
        text-align: center;
    }

    body {
        font-size: 18px;
        font-family: fontHeader;
        color: #dfdfdfef;
        margin: auto;
        padding: auto;
        box-sizing: border-box;
        background: rgba(0, 0, 0, 0.925); 
        display: flex;
        flex-direction: column;
        min-height: 100vh;
        text-align: center; 
    }

    body::before {
        content: "";
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-image: url('fond.jpg');
        background-size: cover; 
        z-index: -1; 
        opacity: 0.5; 
    }

    article {
        background-color: rgba(0, 0, 0, 0.5);
        color: #dfdfdfef;
        padding: 1em;
        margin: 1em 0;
        border-radius: 10px;
        text-align: center; 
    }

    article h2 {
        font-size: 24px;
        margin: 0 0 0.5em;
    }

    article p {
        margin: 0.5em 0;
    }

    article a {
        color: #dfdfdfef;
        text-decoration: none;
        font-weight: bold;
    }
