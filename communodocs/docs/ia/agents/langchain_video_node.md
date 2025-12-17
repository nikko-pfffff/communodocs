```python
def video_node(state: State) -> str:
    """
    """

    prompt = f"""
    """

    if re.search(r'youtube\.com', state["question"]):
        # More flexible regex pattern to match YouTube URLs
        regex_result = re.search(r"(?P<youtube_url>https://(?:www\.)?youtube\.com/watch\?v=[a-zA-Z0-9_-]+)", state["question"])
        if regex_result:
            video_url = regex_result.group("youtube_url")
            downloaded_video = download_youtube_content(url=video_url)
        else:
            # Fallback if regex doesn't match
            print("Could not extract YouTube URL from question. Using question as fallback.")
            downloaded_video = state["downloaded_file"]
    else:
        downloaded_video = state["downloaded_file"]

    print(f"Downloaded video: {downloaded_video}")

    video_mime_type = "video/mp4"

    with open(downloaded_video, "rb") as video_file:
        encoded_video = base64.b64encode(video_file.read()).decode("utf-8")

    os.remove(downloaded_video)

    message = [
        {
            "role": "user",
            "content": [
                {
                    "type": "text",
                    "text": prompt,
                },
                {
                    "type": "media",
                    "data": encoded_video,  # Use base64 string directly
                    "mime_type": video_mime_type,
                },
            ]
        }
    ]

    video_node_response = [video_handler_model.invoke(
        input=message,
        # config={
        #     "callbacks": [langfuse_handler]
        # }
    )]

    video_node_response[-1].pretty_print()

    return {
        "video_node_result": video_node_response
    }
``` 