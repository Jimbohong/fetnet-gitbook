# ImageUpload

圖片上傳模組

![image-upload](../images/image-upload.png)

### Usage
```jsx
import { Grid } from '@material-ui/core';
import ImageUpload from '../../components/form/ImageUpload';

class Identity extends React.Component {

  addFile = (name, file) => {
    let cardForm = Object.assign(this.state.cardForm);
    cardForm[name].value = Object.assign(file[0], {
      preview: URL.createObjectURL(file[0]),
    });

    this.setState({
      cardForm,
    });
  };

  render() {
    return (
      <Grid container spacing={2} className='fui-upload-container'>
        <Grid item xs={12} sm={12} md={6}>
          <ImageUpload
            placeholder='可拖曳證件正面至此區塊'
            name='secondary_card_1'
            addFile={(files) => this.addFile('secondary_card_1', files)}
            files={[cardForm.secondary_card_1.value]}
          />
        </Grid>
      </Grid>
    )
  }
}
```

### Sources
```jsx
import React from 'react';
import Dropzone from 'react-dropzone';
import Button from '../Button';
import PropTypes from 'prop-types';

// for profile picture
class ImageUpload extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      hasError: this.props.name === 'identity_image_2' && !!this.props.files[0],
      files: this.props.files,
    };
  }

  componentWillUnmount() {
    // Make sure to revoke the data uris to avoid memory leaks
    if (!!this.state.files[0]) this.state.files.forEach((file) => URL.revokeObjectURL(file.preview));
  }

  componentDidUpdate = (prevProps) => {
    if (prevProps.files !== this.props.files)
      this.setState({
        hasError: this.props.name === 'identity_image_2' && !!this.props.files[0],
        files: this.props.files,
      });
  };

  onDrop = (accepted, rejected) => {
    if (Object.keys(rejected).length !== 0) {
      const message = 'Please submit valid file type';
      this.setState({ warningMsg: message });
    } else {
      this.props.addFile(accepted);
      this.setState({ warningMsg: '' });

      var blobPromise = new Promise((resolve, reject) => {
        const reader = new window.FileReader();
        reader.readAsDataURL(accepted[0]);
        reader.onloadend = () => {
          const base64data = reader.result;
          resolve(base64data);
        };
      });
      blobPromise.then((value) => {
        // console.log(value);
      });
    }
  };

  render() {
    const { files } = this.props;

    return (
      <div className={`fui-dropzone ${this.state.hasError ? 'has-error' : ''}`}>
        <Dropzone onDrop={this.onDrop} multiple={false} accept='image/jpg,image/jpeg,image/png'>
          {({ getRootProps, getInputProps }) => (
            <div {...getRootProps()}>
              <input {...getInputProps()} name={this.props.name} accept='image/x-png,image/png,image/jpeg' />
              {!!files[0] ? (
                <div className='upload-preview'>
                  {this.state.hasError ? (
                    <span className='error'>
                      <i className='icon-error'></i>
                      <span className='text'>{'圖片無法辨識，可能是因為反光、陰影、圖片歪斜遮住了證件內容。'}</span>
                    </span>
                  ) : (
                    <span className='check'>
                      <i className='icon-check'></i>
                    </span>
                  )}
                  <div
                    className='preview-img'
                    style={{ backgroundImage: !!files[0] ? `url(${files[0].preview})` : 'null' }}></div>
                  <Button btnStyle='secondary'>重新上傳</Button>
                </div>
              ) : (
                <div className='upload-description'>
                  <img src={'/resources/common/images/upload@2x.png'} width='120' alt='upload' />
                  <p>{this.props.placeholder}</p>
                  <Button btnStyle='secondary'>上傳圖片</Button>
                </div>
              )}
            </div>
          )}
        </Dropzone>
      </div>
    );
  }
}

ImageUpload.propTypes = {
  name: PropTypes.string,
  placeholder: PropTypes.string,
  files: PropTypes.array,
  addFile: PropTypes.func,
};

export default ImageUpload;
```

### Properties

| 名稱 | 屬性 | 必填 | 選項 | 說明 |
| :--- | :--- | :--- | :--- | :--- |
| name | String | true |  | 欄位名稱 |
| placeholder | String | true |  | 說明文字 |
| files | Array |  |  | 要上傳的預覽圖 |
| addFile | Function |  |  | 選擇圖片後由外部進行上傳，並透過 files 顯示預覽 |