<!DOCTYPE html>
<html lang="zh - CN">
<head>
    <meta charset="UTF - 8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>学术文献搜索</title>

    <style>

        body {
            font-family: Arial, sans - serif;
            background-color: #121212; 
            color: #ffffff; 
            display: flex;
            justify-content: center; 
            min-height: 10vh; 
            margin: 0;
            letter-spacing: 0em;
            margin-top: 12%; 
        }

        h1 {
            font-size: 48px; 
            font-style: italic; 
            position: relative;
            display: inline-block;
            transition: transform 0.3s ease; 
        }

        h1::after {
            content: "";
            position: absolute;
            left: -10px;
            bottom: 5px;
            width: 110%;
            height: 25px; 
            background-color: #007BFF; 
            z-index: -1;
        }

        .search-container {
            text-align: center;
        }

        input {
            width: 310px;
            padding: 10px;
            margin-bottom: 20px;
            background-color: #222222; 
            border: 1px solid #444444; 
            color: #ffffff; 
            position: absolute; 
            display: flex; 
            left: 34.5%; 
        }

        button {
            padding: 9.5px 20px;
            background-color: #007BFF;
            color: white;
            border: none;
            cursor: pointer;
            position: absolute; 
            display: flex; 
            right: 38%; 
        }

        .website-select {
            margin-bottom: 40px;
            display: flex; 
            justify-content: center; 
            align-items: center; 
        }

        .website-select input[type="radio"] {
            position: absolute;
            opacity: 0;
            cursor: pointer;
        }

        .website-select label {
            position: relative;
            padding-left: 28px; 
            margin-right: 20px; 
            cursor: pointer; 
            transition: transform 0.3s ease; 
        }

        .website-select label::before {
            content: "";
            position: absolute;
            left: 0;
            top: 0;
            width: 20px;
            height: 20px;
            border: 1px solid #444444;
            background-color: #222222;
            border-radius: 50%;
        }

        .website-select input[type="radio"]:checked + label::after {
            content: "";
            position: absolute;
            left: 6px;
            top: 6px;
            width: 10px;
            height: 10px;
            background-color: #007BFF;
            border-radius: 50%;
        }

        .website-select label:hover {
            transform: scale(1.1); 
        }

        .loading-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.8);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 9999;
        }

        .loading-text {
            font-size: 50px;
        }

        .vertical-text1,
        .vertical-text2 {
            writing-mode: vertical-rl; 
            font-weight: 500; 
            font-size: 12.0px; 
            line-height: 1.8; 
            letter-spacing: 1px;
            margin-left: 40px; 
            margin-bottom: 4px; 
            display: inline-block;
            color: #808080;
            opacity: 0; 
            transform: translateX(-50px) scale(0.5); 
            transition: opacity 0.3s ease, transform 0.3s ease; 
        }

        .vertical-text2 {
            margin-left: -10px; 
            margin-bottom: -8px; 
        }

        h1:hover {
            transform: scale(1.1); 
        }

        h1:hover + .vertical-text1,
        h1:hover + .vertical-text1 + .vertical-text2 {
            opacity: 1; 
            transform: translateX(0) scale(1); 
        }

        footer {
            position: fixed;
            bottom: 250px;
            left: 0;
            width: 100%;
            background-color: transparent;
            color: #808080;
            text-align: left; 
            font-size: 13px; 
            padding: 0px 37%;
        }

    </style>
</head>
<body>

    <div id="loadingOverlay" class="loading-overlay">
        <div class="loading-text">loading...</div>
    </div>

    <div class="search-container">
        <h1>学术文献搜索</h1>

        <span class="vertical-text1">无需登录</span>
        <span class="vertical-text2">自由复制</span>

        <div class="website-select">

            <input type="radio" id="lunwenf" name="website" value="http://www.lunwenf.com/" checked>
            <label for="lunwenf">论文网</label>

            <input type="radio" id="lunwendata" name="website" value="https://www.lunwendata.com/">
            <label for="lunwendata">中国论文网</label>

            <input type="radio" id="fx361" name="website" value="https://www.fx361.cc/">
            <label for="fx361">参考网</label>

            <input type="radio" id="qikanchina" name="website" value="https://www.qikanchina.com/">
            <label for="qikanchina">期刊网</label>

            <input type="radio" id="hanspub" name="website" value="https://www.hanspub.org/">
            <label for="hanspub">汉斯出版社</label>

            <input type="radio" id="ablesci" name="website" value="https://www.ablesci.com/">
            <label for="ablesci">科研通</label>

            <input type="radio" id="yjbys" name="website" value="https://www.yjbys.com/biyelunwen/">
            <label for="yjbys">应届毕业生网</label>

        </div>
        <form id="searchForm">
            <input type="text" id="searchInput" placeholder="请输入搜索内容">
            <button type="submit">搜索</button>
        </form>
    </div>

    <footer>
        Copyright &copy 2023-2025 KentStudio. All rights reserved. 
    </footer>

    <script>

        window.addEventListener('load', function () {
            const loadingOverlay = document.getElementById('loadingOverlay');

            loadingOverlay.style.display = 'flex';

            setTimeout(function () {
                loadingOverlay.style.display = 'none';
            }, 1000);
        });


        document.getElementById('searchForm').addEventListener('submit', function (e) {
            e.preventDefault();
            const searchText = document.getElementById('searchInput').value;

            if (searchText.trim() === "") {
                alert("请输入搜索内容！");
                return;
            }

            const selectedWebsite = document.querySelector('input[name="website"]:checked').value;
            const newUrl = `https://cn.bing.com/search?q=site:${selectedWebsite} ${searchText}`;
            window.open(newUrl, '_blank');
        });

    </script>
</body>
</html>
