--- 
draft : false
date : 2024-01-08T14:37:18+01:00
title : "TikTok Nextjs Appwrite Schema"
showTableOfContents : true
type: "post"
---

## Learn how to build this!

If you'd like a step-by-step guide on how to build this just [CLICK THE LINK](https://www.youtube.com/watch?v=7zLyVzItZlQ)

![Tiktok Next JS](/images/20240206tiktok_nextjs_youtube.png)

## App Setup (localhost)

```
git clone https://github.com/John-Weeks-Dev/tiktok-clone-nextjs.git

cp .env.example .env
```

You'll have to set up an AppWrite account, and then add all of the details into your .env file.

## AppWrite Schema

### Database Name: tiktok-clone

### Profile Collection:
| Key | Type |
| --- | --- |
| `Document ID` | String |
| `image` | String |
| `bio` | String |
| `user_id` | String |
| `name` | String |

Profile Indexes:
| KEY           | TYPE          | ATTRIBUTE     | ASC/DESC      |
| ------------- | ------------- | ------------- | ------------- |
| user_id       | key           | user_id       | asc           |
| name          | fulltext      | name          | asc           |

Profile Settings (Update Permissions):
| Add Role      | PERMISSIONS   |
| ------------- | ------------- |
| All guests    | Read          |
| All users     | Create, Read, Update, Delete |

### Post Collection:
| Key | Type |
| --- | --- |
| `Document ID` | String |
| `user_id` | String |
| `video_url` | String |
| `text` | String |
| `created_at` | String |
    
Post Indexes:
| KEY           | TYPE          | ATTRIBUTE     | ASC/DESC      |
| ------------- | ------------- | ------------- | ------------- |
| user_id       | key           | user_id       | asc           |

Profile Settings (Update Permissions):
| Add Role      | PERMISSIONS   |
| ------------- | ------------- |
| All guests    | Read          |
| All users     | Create, Read, Update, Delete |

### Like Collection:
| Key | Type |
| --- | --- |
| `Document ID` | String |
| `user_id` | String |
| `post_id` | String |

Like Indexes: 
| KEY           | TYPE          | ATTRIBUTE     | ASC/DESC      |
| ------------- | ------------- | ------------- | ------------- |
| user_id       | key           | user_id       | asc           |
| id            | unique        | id            | asc           |
| post_id       | key           | post_id       | asc           |

Like Settings (Update Permissions):
| Add Role      | PERMISSIONS   |
| ------------- | ------------- |
| All guests    | Read          |
| All users     | Create, Read, Update, Delete |

### Comment Collection:
| Key | Type |
| --- | --- |
| `Document ID` | String |
| `user_id` | String |
| `post_id` | String |
| `text` | String |
| `created_at` | String |
    
Comment Indexes:
| KEY           | TYPE          | ATTRIBUTE     | ASC/DESC      |
| ------------- | ------------- | ------------- | ------------- |
| post_id       | key           | post_id       | asc           |

Comment Settings (Update Permissions):
| Add Role      | PERMISSIONS   |
| ------------- | ------------- |
| All guests    | Read          |
| All users     | Create, Read, Update, Delete |


Once you've connected your application to AppWrite. Run the commands.
    
```
npm i

npm run dev
```

You should be good to go! If you need any more help, take a look at the tutorial video by clicking the link above.

## .env File

The `.env` file should contain the following:

```
NEXT_PUBLIC_APPWRITE_URL='https://cloud.appwrite.io/v1'
NEXT_PUBLIC_ENDPOINT='...'
NEXT_PUBLIC_DATABASE_ID='...'

NEXT_PUBLIC_COLLECTION_ID_PROFILE='...'
NEXT_PUBLIC_COLLECTION_ID_POST='...'
NEXT_PUBLIC_COLLECTION_ID_LIKEL='...'
NEXT_PUBLIC_COLLECTION_ID_COMMENT='...'

NEXT_PUBLIC_BUCKET_NAME='...'
NEXT_PUBLIC_BUCKET_ID='...'
NEXT_PUBLIC_PLACEHOLDER_DEFAULT_IMAGE_ID='...'
```


