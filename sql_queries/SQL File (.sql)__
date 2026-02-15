                        #           OBJECTIVE QUESTION
#Q1.for NULL value 
SELECT *
  FROM users
  WHERE id IS NULL
   OR username IS NULL
   OR created_at IS NULL;
#for DUPLICATE value 
SELECT id, COUNT(*)
FROM users
GROUP BY id
HAVING COUNT(*) > 1;

#Q2.
SELECT activity_level, COUNT(*) AS user_count
FROM (
    SELECT u.id,
    CASE
        WHEN (COUNT(DISTINCT p.id)
            + COUNT(DISTINCT l.photo_id)
            + COUNT(DISTINCT c.id)) = 0 THEN 'Inactive'

        WHEN (COUNT(DISTINCT p.id)
            + COUNT(DISTINCT l.photo_id)
            + COUNT(DISTINCT c.id)) BETWEEN 1 AND 10 THEN 'Low Activity'

        WHEN (COUNT(DISTINCT p.id)
            + COUNT(DISTINCT l.photo_id)
            + COUNT(DISTINCT c.id)) BETWEEN 11 AND 50 THEN 'Medium Activity'

        ELSE 'High Activity'
    END AS activity_level
    FROM users u
    LEFT JOIN photos p ON u.id = p.user_id
    LEFT JOIN likes l ON u.id = l.user_id
    LEFT JOIN comments c ON u.id = c.user_id
    GROUP BY u.id
) t
GROUP BY activity_level
ORDER BY user_count DESC;


#Q3.
SELECT ROUND(AVG(tag_count), 2) AS avg_tags_per_post
FROM (
    SELECT pt.photo_id, COUNT(pt.tag_id) AS tag_count
    FROM photo_tags pt
    GROUP BY pt.photo_id
) t;


#Q4.
SELECT 
    u.id AS user_id,
    u.username,
    COUNT(DISTINCT l.photo_id) AS total_likes,
    COUNT(DISTINCT c.id) AS total_comments,
    (COUNT(DISTINCT l.photo_id) + COUNT(DISTINCT c.id)) AS total_engagement
FROM users u
JOIN photos p ON u.id = p.user_id
LEFT JOIN likes l ON p.id = l.photo_id
LEFT JOIN comments c ON p.id = c.photo_id
GROUP BY u.id, u.username
ORDER BY total_engagement DESC;


#Q5.
#SQL QUERY FOR FOLLOWERS:-
SELECT 
    u.id AS user_id,
    u.username,
    COUNT(f.follower_id) AS total_followers
FROM users u
JOIN follows f 
    ON u.id = f.followee_id
GROUP BY u.id, u.username
ORDER BY total_followers DESC
LIMIT 10;

#SQL QUERY FOR FOLLOWING :-



SELECT 
    u.id AS user_id,
    u.username,
    COUNT(f.followee_id) AS total_followings
FROM users u
JOIN follows f 
    ON u.id = f.follower_id
GROUP BY u.id, u.username
ORDER BY total_followings DESC
LIMIT 10;


#Q6.
SELECT 
    u.id AS user_id,
    u.username,
    COUNT(DISTINCT p.id) AS total_posts,
    COUNT(DISTINCT l.photo_id) AS total_likes,
    COUNT(DISTINCT c.id) AS total_comments,
    ROUND(
        (COUNT(DISTINCT l.photo_id) + COUNT(DISTINCT c.id)) 
        / COUNT(DISTINCT p.id),
        2
    ) AS avg_engagement_per_post
FROM users u
JOIN photos p 
    ON u.id = p.user_id
LEFT JOIN likes l 
    ON p.id = l.photo_id
LEFT JOIN comments c 
    ON p.id = c.photo_id
GROUP BY u.id, u.username
ORDER BY avg_engagement_per_post DESC;



#Q7.
SELECT 
    u.id AS user_id,
    u.username
FROM users u
LEFT JOIN likes l
    ON u.id = l.user_id
WHERE l.user_id IS NULL;


#Q8.
SELECT 
    t.tag_name,
    ROUND(AVG(tag_usage), 2) AS avg_tag_usage
FROM (
    SELECT 
        pt.tag_id,
        COUNT(pt.photo_id) AS tag_usage
    FROM photo_tags pt
    GROUP BY pt.tag_id
) sub
JOIN tags t 
    ON sub.tag_id = t.id
GROUP BY t.tag_name
ORDER BY avg_tag_usage DESC;


#Q9.
SELECT 
    t.tag_name,
    COUNT(DISTINCT u.id) AS active_users,
    COUNT(p.id) AS total_posts
FROM users u
JOIN photos p 
    ON u.id = p.user_id
JOIN photo_tags pt 
    ON p.id = pt.photo_id
JOIN tags t 
    ON pt.tag_id = t.id
GROUP BY t.tag_name
ORDER BY total_posts DESC;


#Q10.
SELECT 
    u.id AS user_id,
    u.username,
    COUNT(DISTINCT l.photo_id) AS total_likes,
    COUNT(DISTINCT c.id) AS total_comments,
    COUNT(DISTINCT pt.tag_id) AS total_photo_tags
FROM users u
JOIN photos p 
    ON u.id = p.user_id
LEFT JOIN likes l 
    ON p.id = l.photo_id
LEFT JOIN comments c 
    ON p.id = c.photo_id
LEFT JOIN photo_tags pt 
    ON p.id = pt.photo_id
GROUP BY u.id, u.username
ORDER BY u.username;



#Q11.
SELECT 
    u.id AS user_id,
    u.username,
    COUNT(DISTINCT l.photo_id) AS total_likes,
    COUNT(DISTINCT c.id) AS total_comments,
    (COUNT(DISTINCT l.photo_id) + COUNT(DISTINCT c.id)) AS total_engagement
FROM users u
JOIN photos p 
    ON u.id = p.user_id
LEFT JOIN likes l 
    ON p.id = l.photo_id
LEFT JOIN comments c 
    ON p.id = c.photo_id
GROUP BY u.id, u.username
ORDER BY total_engagement DESC;



#12.
WITH hashtag_avg_likes AS (
    SELECT
        t.tag_name,
        COUNT(l.photo_id) / COUNT(DISTINCT p.id) AS avg_likes
    FROM tags t
    JOIN photo_tags pt
        ON t.id = pt.tag_id
    JOIN photos p
        ON pt.photo_id = p.id
    LEFT JOIN likes l
        ON p.id = l.photo_id
    GROUP BY t.tag_name
)
SELECT
    tag_name,
    ROUND(avg_likes, 2) AS avg_likes
FROM hashtag_avg_likes
ORDER BY avg_likes DESC;


#Q13.
SELECT 
    u1.id AS user_id,
    u1.username AS user_name,
    u2.id AS followed_user_id,
    u2.username AS followed_user_name
FROM follows f1
JOIN follows f2
    ON f1.follower_id = f2.followee_id
   AND f1.followee_id = f2.follower_id
JOIN users u1
    ON f1.follower_id = u1.id
JOIN users u2
    ON f1.followee_id = u2.id
WHERE f1.follower_id < f1.followee_id;



#                                SUBJECTIVE QUESTION 
#Q1.alterSELECT 
    u.id AS user_id,
    u.username,
    COUNT(DISTINCT p.id) AS total_posts,
    COUNT(DISTINCT l.photo_id) AS total_likes,
    COUNT(DISTINCT c.id) AS total_comments,
    (COUNT(DISTINCT l.photo_id) + COUNT(DISTINCT c.id)) AS total_engagement,
    CASE
        WHEN (COUNT(DISTINCT l.photo_id) + COUNT(DISTINCT c.id)) >= 300 
            THEN 'Gold Reward – Featured Profile & Exclusive Benefits'
        WHEN (COUNT(DISTINCT l.photo_id) + COUNT(DISTINCT c.id)) >= 150 
            THEN 'Silver Reward – Early Access & Bonus Visibility'
        ELSE 
            'Bronze Reward – Loyalty Badge'
    END AS reward_category
FROM users u
LEFT JOIN photos p 
    ON u.id = p.user_id
LEFT JOIN likes l 
    ON p.id = l.photo_id
LEFT JOIN comments c 
    ON p.id = c.photo_id
GROUP BY u.id, u.username
ORDER BY total_engagement DESC;



#Q2.
SELECT 
    u.id AS user_id,
    u.username
FROM users u
LEFT JOIN photos p 
    ON u.id = p.user_id
LEFT JOIN likes l 
    ON u.id = l.user_id
LEFT JOIN comments c 
    ON u.id = c.user_id
WHERE p.id IS NULL
  AND l.user_id IS NULL
  AND c.id IS NULL;




#Q3.
WITH likes_per_post AS (
    SELECT 
        photo_id,
        COUNT(*) AS likes_cnt
    FROM likes
    GROUP BY photo_id
),
comments_per_post AS (
    SELECT 
        photo_id,
        COUNT(*) AS comments_cnt
    FROM comments
    GROUP BY photo_id
),
post_engagement AS (
    SELECT
        p.id AS photo_id,
        t.tag_name,
        COALESCE(l.likes_cnt, 0) + COALESCE(c.comments_cnt, 0) AS engagement_per_post
    FROM photos p
    JOIN photo_tags pt 
        ON p.id = pt.photo_id
    JOIN tags t 
        ON pt.tag_id = t.id
    LEFT JOIN likes_per_post l 
        ON p.id = l.photo_id
    LEFT JOIN comments_per_post c 
        ON p.id = c.photo_id
)

SELECT
    tag_name,
    ROUND(AVG(engagement_per_post), 2) AS avg_engagement_rate
FROM post_engagement
GROUP BY tag_name
ORDER BY avg_engagement_rate DESC;


#Q4.
WITH post_level AS (
    SELECT
        p.id AS photo_id,
        p.user_id,
        COUNT(DISTINCT l.user_id) AS likes_per_post,
        COUNT(DISTINCT c.id) AS comments_per_post
    FROM photos p
    LEFT JOIN likes l 
        ON p.id = l.photo_id
    LEFT JOIN comments c 
        ON p.id = c.photo_id
    GROUP BY p.id, p.user_id
)

SELECT
    u.id AS user_id,
    u.username,
    COUNT(pl.photo_id) AS total_posts,
    ROUND(AVG(pl.likes_per_post), 2) AS avg_likes_per_post,
    ROUND(AVG(pl.comments_per_post), 2) AS avg_comments_per_post,
    ROUND(AVG(pl.likes_per_post + pl.comments_per_post), 2) AS avg_engagement_per_post
FROM users u
LEFT JOIN post_level pl
    ON u.id = pl.user_id
GROUP BY u.id, u.username
ORDER BY avg_engagement_per_post DESC;



#Q5.
WITH follower_count AS (
    SELECT 
        followee_id AS user_id,
        COUNT(*) AS total_followers
    FROM follows
    GROUP BY followee_id
),
post_level AS (
    SELECT
        p.id AS photo_id,
        p.user_id,
        COUNT(DISTINCT l.user_id) AS likes_per_post,
        COUNT(DISTINCT c.id) AS comments_per_post
    FROM photos p
    LEFT JOIN likes l 
        ON p.id = l.photo_id
    LEFT JOIN comments c 
        ON p.id = c.photo_id
    GROUP BY p.id, p.user_id
),
avg_engagement AS (
    SELECT
        user_id,
        ROUND(AVG(likes_per_post + comments_per_post), 2) AS avg_engagement_per_post
    FROM post_level
    GROUP BY user_id
)

SELECT
    u.id AS user_id,
    u.username,
    fc.total_followers,
    ae.avg_engagement_per_post
FROM users u
JOIN follower_count fc 
    ON u.id = fc.user_id
JOIN avg_engagement ae 
    ON u.id = ae.user_id
ORDER BY fc.total_followers DESC, ae.avg_engagement_per_post DESC;

#Q6.
WITH post_level AS (
    SELECT
        p.id AS photo_id,
        p.user_id,
        COUNT(DISTINCT l.user_id) AS likes_per_post,
        COUNT(DISTINCT c.id) AS comments_per_post,
        (COUNT(DISTINCT l.user_id) + COUNT(DISTINCT c.id)) AS engagement_per_post
    FROM photos p
    LEFT JOIN likes l ON p.id = l.photo_id
    LEFT JOIN comments c ON p.id = c.photo_id
    GROUP BY p.id, p.user_id
),
user_metrics AS (
    SELECT
        u.id AS user_id,
        u.username,
        COUNT(pl.photo_id) AS total_posts,
        ROUND(AVG(pl.engagement_per_post), 2) AS avg_engagement_per_post
    FROM users u
    LEFT JOIN post_level pl ON u.id = pl.user_id
    GROUP BY u.id, u.username
),
segmented_users AS (
    SELECT
        user_id,
        username,
        total_posts,
        avg_engagement_per_post,
        CASE
            WHEN total_posts >= 5 AND avg_engagement_per_post >= 65 
                THEN 'High Value Users'
            WHEN total_posts < 5 AND avg_engagement_per_post >= 65 
                THEN 'Engaged Creators'
            WHEN total_posts >= 5 AND avg_engagement_per_post < 65 
                THEN 'Active Low Engagement Users'
            ELSE 'Inactive Users'
        END AS user_segment
    FROM user_metrics
)

SELECT
    user_segment,
    COUNT(*) AS total_users,
    ROUND(AVG(total_posts), 2) AS avg_posts_per_user,
    ROUND(AVG(avg_engagement_per_post), 2) AS avg_engagement_per_user
FROM segmented_users
GROUP BY user_segment
ORDER BY total_users DESC;



#Q7.
SELECT
    campaign_id,
    impressions,
    clicks,
    conversions,
    ROUND((clicks / NULLIF(impressions, 0)) * 100, 2) AS ctr_percentage,
    ROUND((conversions / NULLIF(clicks, 0)) * 100, 2) AS conversion_rate_percentage
FROM ad_campaigns;



#Q8.
WITH post_level AS (
    SELECT
        p.id AS photo_id,
        p.user_id,
        COUNT(DISTINCT l.user_id) AS likes_per_post,
        COUNT(DISTINCT c.id) AS comments_per_post,
        (COUNT(DISTINCT l.user_id) + COUNT(DISTINCT c.id)) AS engagement_per_post
    FROM photos p
    LEFT JOIN likes l 
        ON p.id = l.photo_id
    LEFT JOIN comments c 
        ON p.id = c.photo_id
    GROUP BY p.id, p.user_id
),
user_metrics AS (
    SELECT
        u.id AS user_id,
        u.username,
        COUNT(pl.photo_id) AS total_posts,
        ROUND(AVG(pl.engagement_per_post), 2) AS avg_engagement_per_post
    FROM users u
    LEFT JOIN post_level pl
        ON u.id = pl.user_id
    GROUP BY u.id, u.username
)

SELECT
    user_id,
    username,
    total_posts,
    avg_engagement_per_post
FROM user_metrics
WHERE total_posts >= 3
  AND avg_engagement_per_post >= 65
ORDER BY avg_engagement_per_post DESC;




#Q10.
UPDATE User_Interactions
SET Engagement_Type = 'Heart'
WHERE Engagement_Type = 'Like';




















