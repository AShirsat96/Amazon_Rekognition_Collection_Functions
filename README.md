# AWS Rekognition Collection Management

A Python implementation for managing AWS Rekognition collections, including creation, deletion, and face management within collections.

## Prerequisites

- Python 3.12.3
- AWS Account with Rekognition access
- AWS Access Key and Secret Key
- boto3 library
- S3 bucket for storing images

## Installation

Install the required AWS SDK:
```bash
pip install boto3
```

## Configuration

```python
aws_accesskey = "Your Access Key"
aws_secretaccess = "Your Secret Access Key"
myregion = "your-region"

# Initialize Rekognition client
rekognition_client = boto3.client('rekognition',
    aws_access_key_id=aws_accesskey,
    aws_secret_access_key=aws_secretaccess,
    region_name=myregion
)
```

## Features

### 1. Create Collection
```python
def Create_Collection(aws_access, aws_secret, aws_region, Collection_ID):
    """
    Creates a new Rekognition collection.
    
    Returns:
        Collection ARN and status code
    """
```

### 2. List Collections
```python
def List_Collection(aws_access, aws_secret, aws_region):
    """
    Lists all collections with pagination support.
    
    Returns:
        int: Total number of collections
    """
```

### 3. Delete Collection
```python
def Delete_Collection(aws_access, aws_secret, aws_region, Collection_ID):
    """
    Deletes a specified collection.
    
    Returns:
        int: HTTP status code
    """
```

### 4. Add Faces to Collection
```python
def AddFaces_Collection(aws_access, aws_secret, aws_region, bucket_name, photo_name, Collection_ID):
    """
    Indexes faces from S3 image into collection.
    
    Features:
    - Supports up to 15 faces per image
    - Automatic quality filtering
    - Returns face IDs and locations
    
    Returns:
        int: Number of faces indexed
    """
```

### 5. Describe Collection
```python
def describe_collection(aws_access, aws_secret, aws_region, Collection_ID):
    """
    Retrieves collection metadata including:
    - Collection ARN
    - Face count
    - Face model version
    - Creation timestamp
    """
```

### 6. Delete Faces
```python
def deleteFaces_collection(aws_access, aws_secret, aws_region, Collection_ID, faces):
    """
    Removes specified faces from collection.
    
    Returns:
        int: Number of faces deleted
    """
```

## Usage Examples

### Create and List Collections
```python
# Create collection
Create_Collection(aws_accesskey, aws_secretaccess, myregion, "MyCollection")

# List all collections
collection_count = List_Collection(aws_accesskey, aws_secretaccess, myregion)
```

### Add and Manage Faces
```python
# Add faces from S3 image
faces_added = AddFaces_Collection(
    aws_accesskey,
    aws_secretaccess,
    myregion,
    "bucket_name",
    "image.jpg",
    "MyCollection"
)

# Delete specific faces
faces_to_delete = ["face-id-1", "face-id-2"]
deleted_count = deleteFaces_collection(
    aws_accesskey,
    aws_secretaccess,
    myregion,
    "MyCollection",
    faces_to_delete
)
```

## Response Formats

### Collection Creation
```
Creating collection: MyCollection
Collection ARN: aws:rekognition:region:account:collection/MyCollection
Status code: 200
```

### Face Indexing
```
Results for image.jpg
Faces indexed:
  Face ID: 123456-abcd...
  Location: {'Width': 0.19, 'Height': 0.48, 'Left': 0.50, 'Top': 0.40}
```

## Error Handling

The implementation handles:
- ResourceNotFoundException
- Client errors
- Invalid collection IDs
- Face deletion errors
- S3 access issues

## Limitations

- Maximum 15 faces per image indexing
- Collection naming constraints
- Region-specific availability
- S3 image requirements

## Best Practices

1. Collection Management:
   - Use meaningful collection IDs
   - Clean up unused collections
   - Monitor face counts

2. Face Indexing:
   - Use high-quality images
   - Consider face size and angles
   - Implement proper error handling

3. Performance:
   - Batch face operations when possible
   - Monitor API limits
   - Implement pagination for large collections
