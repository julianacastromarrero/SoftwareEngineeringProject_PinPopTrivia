CREATE DATABASE pinpop;
USE pinpop;

CREATE TABLE IF NOT EXISTS admin (
	admin_id	INT AUTO_INCREMENT,
	full_name	VARCHAR(50) NOT NULL,
    role		VARCHAR(30) NOT NULL DEFAULT 'Admin', 
    email		VARCHAR(50) NOT NULL UNIQUE,
    password	VARCHAR(50) NOT NULL,
    PRIMARY KEY (admin_id)
);

CREATE TABLE IF NOT EXISTS player (
    player_id		INT NOT NULL AUTO_INCREMENT,
    username		VARCHAR(50) NOT NULL UNIQUE,
    email			VARCHAR(50) NOT NULL UNIQUE,
    password		VARCHAR(50) NOT NULL,
    date_join		DATE NOT NULL DEFAULT (CURRENT_DATE),
    freeplay_score	INT NOT NULL DEFAULT 0,
    is_deleted		BOOLEAN NOT NULL DEFAULT FALSE,
    PRIMARY KEY (player_id)
);

CREATE TABLE IF NOT EXISTS media (
	media_id		INT NOT NULL AUTO_INCREMENT,
    title			VARCHAR(50) NOT NULL,
    direct_author	VARCHAR(50),
    media_type		ENUM('Movie','Book', 'TV Show') NOT NULL,
    PRIMARY KEY (media_id)
);

CREATE TABLE IF NOT EXISTS location (
	location_id		INT NOT NULL AUTO_INCREMENT,
    city_name		VARCHAR(50) NOT NULL,
    country			VARCHAR(50) NOT NULL,
    region			ENUM('Africa', 'Oceania', 'Asia', 'Europe', 'North America', 'South America','Middle East') NOT NULL,
    PRIMARY KEY (location_id)
);

CREATE TABLE IF NOT EXISTS questions (
	q_id		INT NOT NULL AUTO_INCREMENT,
    question	VARCHAR(500) NOT NULL,
    media_id	INT,
    location_id	INT NOT NULL,
    answer      ENUM('A','B','C','D') NOT NULL,
    option_a    VARCHAR(100) NOT NULL,
    option_b    VARCHAR(100) NOT NULL,
    option_c    VARCHAR(100) NOT NULL,
    option_d    VARCHAR(100) NOT NULL,
    points      INT NOT NULL DEFAULT 1000,
    date_added  DATE NOT NULL DEFAULT (CURRENT_DATE),
    author      VARCHAR(100),
    PRIMARY KEY (q_id),
    FOREIGN KEY (media_id) REFERENCES media(media_id) ON DELETE SET NULL,
    FOREIGN KEY (location_id) REFERENCES location(location_id) ON DELETE RESTRICT
);

CREATE TABLE IF NOT EXISTS quiz (
	quiz_id		INT NOT NULL AUTO_INCREMENT,
    player_id	INT NOT NULL,
    quiz_type	ENUM('weekly', 'freeplay', 'regional') NOT NULL,
    score		INT NOT NULL DEFAULT 0,
    date		DATE NOT NULL DEFAULT (CURRENT_DATE),
    PRIMARY KEY (quiz_id),
    FOREIGN KEY (player_id) REFERENCES player(player_id) ON DELETE CASCADE
);

CREATE TABLE IF NOT EXISTS quiz_questions (
	quiz_q_id	INT NOT NULL AUTO_INCREMENT,
    quiz_id		INT NOT NULL,
    q_id		INT NOT NULL,
    player_ans	ENUM('A','B','C','D'),
    time_taken	DECIMAL(5,2),
    points_obt	INT NOT NULL DEFAULT 0,
    PRIMARY KEY (quiz_q_id),
    FOREIGN KEY (quiz_id) REFERENCES quiz(quiz_id) ON DELETE CASCADE,
    FOREIGN KEY (q_id) REFERENCES questions(q_id) ON DELETE RESTRICT    
);
    
CREATE TABLE IF NOT EXISTS leaderboard (
    leaderboard_id	INT NOT NULL AUTO_INCREMENT,
    player_id		INT NOT NULL,
    board_type		ENUM('weekly','freeplay') NOT NULL,
    score 			INT NOT NULL DEFAULT 0,
    date			DATE NOT NULL DEFAULT (CURRENT_DATE),
    week_start		DATE,
    PRIMARY KEY (leaderboard_id),
    FOREIGN KEY (player_id) REFERENCES player(player_id) ON DELETE CASCADE
);

CREATE TABLE IF NOT EXISTS Badges (
    badge_id	INT NOT NULL AUTO_INCREMENT,
    player_id	INT NOT NULL,
    badge_type  ENUM('top3_weekly', 'freeplay_board') NOT NULL,
    week_date   DATE,
    awarded_on  DATE,
    PRIMARY KEY (badge_id),
    FOREIGN KEY (player_id) REFERENCES player(player_id) ON DELETE CASCADE
);

CREATE TABLE IF NOT EXISTS contact (
    submission_id	INT NOT NULL AUTO_INCREMENT,
    player_id		INT,
    submission_type ENUM('feedback', 'suggestion', 'other') NOT NULL,
    submitted_date  DATE NOT NULL DEFAULT (CURRENT_DATE),
    week_start      DATE,
    message         VARCHAR(2000), -- for feedback or other submission type
    q_id            INT,
    question_text   VARCHAR(1000),
    option_A        VARCHAR(500),
    option_B        VARCHAR(500),
    option_C        VARCHAR(500),
    option_D        VARCHAR(500),
    correct_option  ENUM('A','B','C','D'),
    status          ENUM('pending', 'approved', 'rejected', 'resolved') NOT NULL DEFAULT 'pending',
    admin_review    INT, -- admin_id
    PRIMARY KEY (submission_id),
    FOREIGN KEY (player_ID) REFERENCES player(player_id) ON DELETE SET NULL,
    FOREIGN KEY (q_id) REFERENCES questions(q_id) ON DELETE SET NULL,
    FOREIGN KEY (admin_review) REFERENCES admin(admin_id) ON DELETE SET NULL,
    UNIQUE KEY weekly_suggestion_player (player_id, week_start)
);


-- PRELIM DATA INSERTION
INSERT INTO admin(full_name, role, email, password) VALUES 
	('Juliana Castro', 'Back end developer', 'jcastro8@trinity.edu', 'pwd'),
    ('Natalie Hudson', 'Full stack developer', 'nhudson@trinity.edu', 'pwd'),
    ('Al Stubblefield','Front end developer','astubble@trinity.edu', 'pwd');

INSERT INTO location(city_name, country, region) VALUES
	('New York', 'USA', 'North America'),
    ('Amsterdam', 'Netherlands', 'Europe'), --
    ('Wellington', 'New Zealand', 'Oceania'), --
    ('London', 'United Kingdom', 'Europe'), --
    ('Tokyo', 'Japan', 'Asia'), --
    ('Tehran', 'Iran', 'Middle East'), --
    ('Buenos Aires', 'Argentina', 'South America'), --
    ('Rio de Janeiro', 'Brazil', 'South America'), --
    ('Mexico City', 'Mexico', 'North America'),
    ('Dubai','UAE','Middle East'),
    ('Paris','France','Europe'),
    ('Seoul','South Korea', 'Asia'),
    ('Sydney', 'Australia','Oceania'),
    ('Seattle','USA','North America'),
    ('Monaco', 'Monaco', 'Europe'),
    ('Kigali', 'Rwanda', 'Africa'),
    ('Johannesburg', 'South Africa', 'Africa'), -- Hotel Rwanda + Avengers Age of Ultron
    ('Cape Town', 'South Africa', 'Africa'), -- Mad Max
    ('Los Angeles', 'USA', 'North America'),
    ('Chicago', 'USA', 'North America'); -- The Bear
    
INSERT INTO media(title, direct_author, media_type) VALUES 
    ('Persepolis', 'Marjane Satrapi', 'Book'), -- Tehran
    ('XOXO Kitty', 'Jenny Han', 'TV Show'), -- Seoul
    ('Lost in Translation', 'Sofia Coppola', 'Movie'), -- Tokyo
    ('Parasite', 'Bong Joon Ho', 'Movie'), -- Seoul
    ('Your Name', 'Makoto Shinkai', 'Movie'), -- Tokyo
    ('The Diary of Anne Frank', 'Anne Frank', 'Book'), -- Amsterdam
    ('The Fault in our Stars', 'John Green', 'Book'), -- Amsterdam
    ('Girl with a Pearl Earring', 'Peter WEbber', 'Movie'), -- Amsterdam
    ('Lord of the Rings', 'Peter Jackson', 'Movie'), -- Wellington
    ('Bridgerton', 'Julia Quinn', 'TV Show'), -- London
    ('Fleabag', 'Phoebe Waller-Bridge', 'TV Show'), -- London
    ('Rio', 'Carlos Saldanha', 'Movie'),
    ('Twilight - Breaking Dawn Part 1', 'Bill Condon', 'Movie'), -- Rio de Janeiro
    ('Roma', 'Alfonso Cuaron', 'Movie'); -- CDMX
    
