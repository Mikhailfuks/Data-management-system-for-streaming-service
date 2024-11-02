CREATE TABLE Users (
    UserId INT PRIMARY KEY IDENTITY(1,1),
    Username VARCHAR(50) NOT NULL,
    Email VARCHAR(255) NOT NULL,
    Password VARCHAR(255) NOT NULL,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    SubscriptionType VARCHAR(50) -- "Free", "Premium"
);

-- Таблица контента
CREATE TABLE Content (
    ContentId INT PRIMARY KEY IDENTITY(1,1),
    Title VARCHAR(255) NOT NULL,
    Description TEXT,
    Type VARCHAR(50) NOT NULL, -- "Movie", "TV Show", "Music", "Podcast"
    Genre VARCHAR(50),
    ReleaseDate DATE,
    Duration INT,
    Rating DECIMAL(2,1),
    ImageUrl VARCHAR(255),
    IsFeatured BOOLEAN,
    IsExclusive BOOLEAN
);

-- Таблица сезонов (для сериалов)
CREATE TABLE Seasons (
    SeasonId INT PRIMARY KEY IDENTITY(1,1),
    ContentId INT NOT NULL,
    SeasonNumber INT NOT NULL,
    NumberOfEpisodes INT NOT NULL,
    FOREIGN KEY (ContentId) REFERENCES Content(ContentId)
);

-- Таблица эпизодов
CREATE TABLE Episodes (
    EpisodeId INT PRIMARY KEY IDENTITY(1,1),
    SeasonId INT NOT NULL,
    EpisodeNumber INT NOT NULL,
    Title VARCHAR(255),
    Duration INT,
    ReleaseDate DATE,
    FOREIGN KEY (SeasonId) REFERENCES Seasons(SeasonId)
);

-- Таблица актеров
CREATE TABLE Actors (
    ActorId INT PRIMARY KEY IDENTITY(1,1),
    ActorName VARCHAR(255) NOT NULL,
    Biography TEXT,
    ImageUrl VARCHAR(255)
);

-- Таблица "Актеры в контенте"
CREATE TABLE ContentActors (
    ContentActorId INT PRIMARY KEY IDENTITY(1,1),
    ContentId INT NOT NULL,
    ActorId INT NOT NULL,
    FOREIGN KEY (ContentId) REFERENCES Content(ContentId),
    FOREIGN KEY (ActorId) REFERENCES Actors(ActorId)
);

-- Таблица "Просмотры"
CREATE TABLE Views (
    ViewId INT PRIMARY KEY IDENTITY(1,1),
    UserId INT NOT NULL,
    ContentId INT NOT NULL,
    ViewDate DATE NOT NULL,
    ViewTime TIME NOT NULL,
    FOREIGN KEY (UserId) REFERENCES Users(UserId),
    FOREIGN KEY (ContentId) REFERENCES Content(ContentId)
);

-- Таблица "Избранное"
CREATE TABLE Favorites (
    FavoriteId INT PRIMARY KEY IDENTITY(1,1),
    UserId INT NOT NULL,
    ContentId INT NOT NULL,
    FOREIGN KEY (UserId) REFERENCES Users(UserId),
    FOREIGN KEY (ContentId) REFERENCES Content(ContentId)
);

-- Таблица "История просмотров"
CREATE TABLE WatchHistory (
    HistoryId INT PRIMARY KEY IDENTITY(1,1),
    UserId INT NOT NULL,
    ContentId INT NOT NULL,
    LastWatchedTimestamp DATETIME NOT NULL,
    Progress INT NOT NULL, -- Процент просмотра
    FOREIGN KEY (UserId) REFERENCES Users(UserId),
    FOREIGN KEY (ContentId) REFERENCES Content(ContentId)
);

-- Пример добавления пользователя
INSERT INTO Users (Username, Email, Password, SubscriptionType) VALUES
('john.doe', 'john.doe@email.com', 'password123', 'Premium');

-- Пример добавления фильма
INSERT INTO Content (Title, Description, Type, Genre, ReleaseDate, Duration, Rating, ImageUrl, IsFeatured, IsExclusive) VALUES
('The Shawshank Redemption', 'A meticulous and gripping tale of wrongful imprisonment and enduring hope.', 'Movie', 'Drama', '1994-09-23', 142, 9.3, 'images/shawshank.jpg', TRUE, FALSE);

-- Пример добавления сезона сериала
INSERT INTO Seasons (ContentId, SeasonNumber, NumberOfEpisodes) VALUES
(2, 1, 10);

-- Пример добавления эпизода
INSERT INTO Episodes (SeasonId, EpisodeNumber, Title, Duration, ReleaseDate) VALUES
(1, 1, 'Pilot', 42, '2023-03-15');

-- Пример добавления актера
INSERT INTO Actors (ActorName, Biography, ImageUrl) VALUES
('Morgan Freeman', 'An acclaimed American actor, known for his deep voice and memorable roles.', 'images/morgan-freeman.jpg');

-- Пример добавления актера к фильму
INSERT INTO ContentActors (ContentId, ActorId) VALUES
(1, 1);

-- Пример добавления просмотра
INSERT INTO Views (UserId, ContentId, ViewDate, ViewTime) VALUES
(1, 1, '2023-03-16', '14:30:00');

-- Пример добавления фильма в "Избранное"
INSERT INTO Favorites (UserId, ContentId) VALUES
(1, 1);

-- Пример добавления истории просмотра
INSERT INTO WatchHistory (UserId, ContentId, LastWatchedTimestamp, Progress) VALUES
(1, 1, '2023-03-16 15:00:00', 50);
