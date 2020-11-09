# Input

## TextInput

文字輸入框，不包含 &lt;label/&gt; 標籤

**Usage**
```jsx
import Formsy from 'formsy-react';
import TextInput from '../components/form/TextInput';


class Login extends React.Component {
  disableButton() {
    this.setState({ canSubmit: false });
  }

  enableButton() {
    this.setState({ canSubmit: true });
  }
  
  render() {
    return (
      <Formsy onValid={this.enableButton} onInvalid={this.disableButton}>
          <TextInput
              placeholder='請輸入統一編號/企業認證碼'
              name='number'
              validations='isLength:8'
              validationError='您輸入的認證碼有誤，請查詢後重新輸入'
              required
          />
      </Formsy>
    )
  }
}
```
---

**TextInput.js**
```jsx
import { withFormsy } from 'formsy-react';
import React from 'react';
import PropTypes from 'prop-types';

class TextInput extends React.Component {
  constructor(props) {
    super(props);
    this.changeValue = this.changeValue.bind(this);
    this.currentValue = null;
    this.state = {
      submitted: false,
      isInvalid: false,
    };
  }

  componentDidUpdate() {
    if (this.props.isFormSubmitted() !== this.state.submitted) {
      this.setState({
        submitted: this.props.isFormSubmitted(),
        isInvalid: this.props.showRequired() || this.props.showError(),
      });
    }
  }

  changeValue(event) {
    this.props.setValue(event.currentTarget.value);
    this.currentValue = event.currentTarget.value;
    if(this.props.onChange)
      this.props.onChange(event.currentTarget.value)
    this.setState({
      isInvalid: this.props.showRequired() || this.props.showError(),
    });
  }

  resetInput = () => {
    this.props.setValue(null);
    this.currentValue = null;
  };

  render() {
    const errorMessage = this.props.getErrorMessage();
    return (
      <div className='text-input'>
        <input
          className={!!errorMessage ? 'invalid' : ''}
          placeholder={this.props.placeholder}
          onChange={this.changeValue}
          type='text'
          value={this.props.getValue() || ''}
        />
        {this.currentValue ? (
          <div className='reset' onClick={this.resetInput}>
            <i className='icon-close'></i>
          </div>
        ) : (
          ''
        )}
        {!!this.state.isInvalid ? <span className='error-message'>{errorMessage}</span> : ''}
      </div>
    );
  }
}

TextInput.propTypes = {
  changeValue: PropTypes.bool,
  currentValue: PropTypes.func,
  onChange: PropTypes.func
};

export default withFormsy(TextInput);

```

#### Properties

| 名稱 | 屬性 | 必填 | 選項 | 說明 |
| :--- | :--- | :--- | :--- | :--- |
| nam | String | true |  | 輸入框名稱 |
| placeholder | String |  |  | 輸入框提示訊息 |
| validations | String or Object |  |  | 驗證條件 |
| validationError | String or Object |  |  | 錯誤訊息 |
| required | Boolean |  |  | 是否為必填 |
| value | String |  |  | 值 |
| onChange | Function |  |  | 輸入框值改變事件 |

## LabelInput

文字輸入框，包含 &lt;label/&gt; 標籤

**Usage**
```jsx
import Formsy from 'formsy-react';
import LabelInput from '../components/form/LabelInput';


class Login extends React.Component {
    constructor(props) {
        super(props);
        this.form = React.createRef();
        this.map = null;
        this.state = {
            canSubmit: true,
            form: {
                email: { value: '', required: true }
            },
        };
        this.onChange = this.onChange.bind(this);
        this.disableButton = this.disableButton.bind(this);
        this.enableButton = this.enableButton.bind(this);
        this.submit = this.submit.bind(this);
    }
    
    disableButton() {
        this.setState({ canSubmit: false });
    }
    
    enableButton() {
        this.setState({ canSubmit: true });
    }
        
    render() {
        return (
          <Formsy onValid={this.enableButton} onInvalid={this.disableButton}>
              <LabelInput
                  validations='isEmail'
                  validationErrors={{
                      isEmail: '請輸入正確的 Email',
                      isDefaultRequiredValue: '請輸入 Email',
                  }}
                  name='email'
                  required={this.state.form.email.required}
                  value={this.state.form.email.value}
                  label='Email'
                  onChange={this.onChange}
              />
          </Formsy>
        )
    }
}
```

**LabelInput.js**
```jsx
import React from 'react';
import { withFormsy } from 'formsy-react';
import PropTypes from 'prop-types';

class LabelInput extends React.Component {
    constructor(props) {
        super(props);
        this.input = React.createRef();
        this.state = {
            submitted: false,
            isInvalid: false,
            required: this.props.required || false,
            placeholder: this.props.placeholder || '',
            maxLength: this.props.maxLength || 200
        }

        this.resetInput = this.resetInput.bind(this)
        this.handleChange = this.handleChange.bind(this)
    }

    componentDidMount() {
        this.input.current.value = this.props.getValue();
    }

    componentDidUpdate(prevProps) {
        // console.group(prevProps.value, 
        if (prevProps.getValue() !== this.props.getValue()) {
            this.input.current.value = this.props.getValue()
        }
        if (this.props.isFormSubmitted() !== this.state.submitted) {
            // console.log(this.props.value)
            this.setState({
                submitted: this.props.isFormSubmitted(),
                isInvalid: this.props.showRequired() || this.props.showError()
            })
        }
    }

    resetInput() {
        this.input.current.value = '';
        this.props.setValue(null);
    }

    handleChange(event) {
        this.props.setValue(event.currentTarget.value);
        this.setState({
            isInvalid: this.props.showRequired() || this.props.showError()
        })
        this.props.onChange(this.props.name, event.currentTarget.value)
    }

    render() {
        const errorMessage = this.props.getErrorMessage();

        return (
            <div className={`form-group ${this.state.isInvalid ? 'is-invalid' : ''}`}>
                {!!this.props.label ? <label
                    htmlFor={`input-${this.props.name}`}
                    className={this.state.required ? 'is-required' : ''}>
                    {this.props.label}
                </label> : ''}
                <div className="text-input">
                    <input
                    onBlur={this.handleChange}
                    id={`input-${this.props.name}`}
                    type="text"
                    ref={this.input}
                    onChange={this.handleChange}
                    onKeyUp={this.handleChange}
                    placeholder={this.state.placeholder}
                    value={this.props.getValue() || ''}
                    maxLength={this.state.maxLength} />
                    {
                        this.props.getValue() 
                        ? <div className="reset" onClick={this.resetInput}><i className="icon-close"></i></div> 
                        : '' 
                    }
                </div>
                {
                    this.state.isInvalid ? (<span className="error-message">{errorMessage}</span>) : ''
                }
            </div>
        )
    }
}

LabelInput.propTypes = {
    required: PropTypes.bool.isRequired,
    label: PropTypes.string.isRequired,
    placeholder: PropTypes.string,
    value: PropTypes.string,
    maxLength: PropTypes.number
}

export default withFormsy(LabelInput);
```

#### Properties

| 名稱 | 屬性 | 必填 | 選項 | 說明 |
| :--- | :--- | :--- | :--- | :--- |
| name | String | true |  | 輸入框名稱 |
| placeholder | String |  |  | 輸入框提示訊息 |
| validations | String or Object |  |  | 驗證條件 |
| validationErrors | String or Object |  |  | 錯誤訊息 |
| required | Boolean |  |  | 是否為必填 |
| label | String |  |  | 欄位說明 |
| value | String |  |  | 值 |
| onChange | Function |  |  | 輸入框值改變事件 |

### LabelPassword

密碼輸入框，用法與 LabelInput 類似。

```jsx
import React from 'react';
import { withFormsy } from 'formsy-react';
import PropTypes from 'prop-types';

class LabelPassword extends React.Component {
  constructor(props) {
    super(props);
    this.input = React.createRef();
    this.state = {
      show: false,
      submitted: false,
      isInvalid: false,
      required: this.props.required || false,
      placeholder: this.props.placeholder || '',
      maxLength: this.props.maxLength || 200,
    };

    this.resetInput = this.resetInput.bind(this);
    this.handleChange = this.handleChange.bind(this);
  }

  componentDidMount() {
    this.input.current.value = this.props.getValue();
  }

  componentDidUpdate(prevProps) {
    // console.group(prevProps.value,
    if (prevProps.getValue() !== this.props.getValue()) {
      this.input.current.value = this.props.getValue();
    }
    if (this.props.isFormSubmitted() !== this.state.submitted) {
      // console.log(this.props.value)
      this.setState({
        submitted: this.props.isFormSubmitted(),
        isInvalid: this.props.showRequired() || this.props.showError(),
      });
    }
  }

  resetInput() {
    this.input.current.value = '';
    this.props.setValue(null);
  }

  handleChange(event) {
    this.props.setValue(this.input.current.value);
    this.setState({
      submitted: this.props.isFormSubmitted(),
      isInvalid: this.props.showRequired() || this.props.showError(),
    });
    this.props.onChange(this.props.name, this.input.current.value);
  }

  render() {
    const errorMessage = this.props.getErrorMessage();

    return (
      <div className={`form-group ${this.state.isInvalid ? 'is-invalid' : ''}`}>
        {!!this.props.label ? (
          <label htmlFor={`input-${this.props.name}`} className={this.state.required ? 'is-required' : ''}>
            {this.props.label}
          </label>
        ) : (
          ''
        )}
        <div className='text-input'>
          <input
            onBlur={this.handleChange}
            id={`input-${this.props.name}`}
            type={this.state.show ? 'text' : 'password'}
            ref={this.input}
            onChange={this.handleChange}
            onKeyUp={this.handleChange}
            placeholder={this.state.placeholder}
            value={this.props.getValue() || ''}
            maxLength={this.state.maxLength}
          />
          <div
            className={`eye ${this.state.show ? 'pwd-show' : 'pwd-hide'}`}
            onClick={(e) => this.setState({ show: !this.state.show })}>
            <i className='icon-hide-view'></i>
          </div>
          {this.props.getValue() ? (
            <div className='reset' onClick={this.resetInput}>
              <i className='icon-close'></i>
            </div>
          ) : null}
        </div>
        {this.state.isInvalid ? <span className='error-message'>{errorMessage}</span> : ''}
      </div>
    );
  }
}

LabelPassword.propTypes = {
  required: PropTypes.bool.isRequired,
  label: PropTypes.string.isRequired,
  placeholder: PropTypes.string,
  value: PropTypes.string,
  maxLength: PropTypes.number,
};

export default withFormsy(LabelPassword);

```
#### Properties

| 名稱 | 屬性 | 必填 | 選項 | 說明 |
| :--- | :--- | :--- | :--- | :--- |
| nam | String | true |  | 輸入框名稱 |
| placeholder | String |  |  | 輸入框提示訊息 |
| required | Boolean |  |  | 是否為必填 |
| label | String |  |  | 欄位說明 |
| validations | String or Object |  |  | 驗證條件 |
| validationErrors | String or Object |  |  | 錯誤訊息 |
| value | String |  |  | 值 |
| onChange | Function |  |  | 輸入框值改變事件 |

--- 

### LabelTextarea

**Desktop**
![](../.gitbook/assets/image%20%2866%29.png)

**Mobile**
![](../.gitbook/assets/image%20%28231%29.png)

**Usage**
```javascript
import LabelTextarea from '../form/LabelTextarea'
<LabelTextarea 
    placeholder='若您對此解答有其他意見，請說明。幫助我們提供更佳服務，謝謝！' 
    label='' 
    name='comment'
    value={this.state.feedbackForm.comment}
    onChange={this.storeForm} 
/>
```

**Source**
```javascript
import React from 'react';
import { withFormsy } from 'formsy-react';
import PropTypes from 'prop-types';

class LabelTextarea extends React.Component {
    constructor(props) {
        super(props);
        this.input = React.createRef();
        this.state = {
            isFocus: false,
            required: this.props.required || false,
            placeholder: this.props.placeholder || '您可以在此輸入需求...',
            maxLength: this.props.maxLength || 450
        }

        this.handleChange = this.handleChange.bind(this)
    }

    componentDidMount() {
        this.input.current.value = this.props.getValue();
    }

    setFocus = event => {
        this.setState({
            isFocus: true
        })
    }

    setBlur = event => {
        this.setState({
            isFocus: false
        })
    }

    handleChange = (event) => {
        let val = event.currentTarget.value
        if (val.length > this.state.maxLength) return false;
        this.setState({value: val})
        this.props.onChange(this.props.name, val)
    }

    render() {
        return (
            <div className={`form-group`}>
                <label htmlFor={`textarea-${this.props.name}`} className={this.state.required ? 'is-required' : ''}>{this.props.label}</label>
                <div className={`fui-textarea ${this.state.isFocus ? 'is-focus' : ''}`}>
                    <textarea 
                    id={`textarea-${this.props.name}`}
                    name={this.props.name} 
                    ref={this.input} 
                    placeholder={this.props.placeholder} 
                    value={this.props.getValue() || ''}
                    onBlur={this.setBlur}
                    onFocus={this.setFocus}
                    onKeyPress={this.handleChange} 
                    onChange={this.handleChange}></textarea>
                    <div className="max-length-counter">
                        {(this.props.getValue()).length} / {this.state.maxLength}
                    </div>
                </div>
            </div>
        )
    }
}

LabelTextarea.propTypes = {
    required: PropTypes.bool,
    label: PropTypes.string.isRequired,
    name: PropTypes.string.isRequired,
    placeholder: PropTypes.string,
    value: PropTypes.string,
    maxLength: PropTypes.number,
    onChange: PropTypes.func
}

export default withFormsy(LabelTextarea);
```
{% endtab %}
{% endtabs %}



#### Properties

| 名稱 | 屬性 | 必填 | 選項 | 說明 |
| :--- | :--- | :--- | :--- | :--- |
| required | bool |  |  | 選填或必填 |
| label | string | true |  | 標題 |
| name | string | true |  | 表單名稱，不能重複命名 |
| placeholder | string |  |  | 預設顯示 |
| value | string |  |  | 填入的值 |
| maxLength | number |  |  | 上限長度 |
| onChange | func |  |  | 觸發事件 |

### KeywordInput
**Desktop**
![](../.gitbook/assets/image%20%28232%29.png)

**Mobile**
![](../.gitbook/assets/image%20%2851%29.png)

**Usage**
```javascript
import KeywordInput from '../../KeywordInput';

<KeywordInput
  title={mock.keywordInput}
  defaultKeyword={[
    'Gogoro',
    'Gogoro 699',
    'google',
    'GPS',
    'GPS Google',
    'Gogo',
    'GoPro',
    'Geo',
    'SEO',
    'OPPO',
  ]}
/>
```

**KeywordInput.js**
```javascript
import React from 'react';
import Autocomplete from 'react-autocomplete';
import Button from './Button';
import PropTypes from 'prop-types';

class KeywordInput extends React.Component {
  constructor(props) {
    super(props);
    this.input = React.createRef();
    this.state = {
      isEBU: window.location.pathname.indexOf('ebu') > -1,
      keywordMenu: this.props.defaultKeyword.slice(0, 6),
      keyword: this.props.keyword || '',
    };

    this.doSearch = this.doSearch.bind(this);
  }

  componentDidMount() {
    this.setState({
      keyword: this.props.keyword || '',
    });
  }

  highlightKeyword(word) {
    let value = this.state.keyword.toLowerCase();
    let index = word.toLowerCase().indexOf(value);

    if (index === -1 || value.value === '') {
      return word;
    } else {
      // console.log(word.substring(0,index) + "<span class='highlight'>" + word.substring(index,index+value.length) + "</span>" + word.substring(index + value.length))
      return (
        word.substring(0, index) +
        "<span class='highlight'>" +
        word.substring(index, index + value.length) +
        '</span>' +
        word.substring(index + value.length)
      );
    }
  }

  resetInput = () => {
    this.setState({
      keyword: '',
    });
  };

  doSearch(text) {
    window.location.href = `${this.props.path || '/ebu/search'}?keyword=${text || this.state.keyword}`;
  }

  render() {
    return (
      <div className='keyword-input-container'>
        {this.props.title ? <h2>{this.props.title}</h2> : ''}
        <Autocomplete
          wrapperStyle={{ width: '100%', position: 'relative' }}
          getItemValue={item => item}
          items={this.state.keywordMenu}
          value={this.state.keyword}
          onKeyDowe={this.inputChange}
          renderInput={props => (
            <div className={`fui-input-group is-keyword`}>
              <div className='fui-input'>
                <input ref={this.input} {...props} type='text' placeholder={this.props.placeholder || '立即搜尋 IOT'} />
                {props.value !== '' ? (
                  <div className='reset' onClick={this.resetInput}>
                    <i className='icon-close'></i>
                  </div>
                ) : (
                  ''
                )}
              </div>
              <div className='fui-action'>
                <Button onClick={this.doSearch} btnStyle='primary'>
                  搜尋
                </Button>
              </div>
            </div>
          )}
          onSelect={(value, item) => {
            this.setState({ keyword: item });
          }}
          onChange={(event, value) => {
            this.setState({ keywordMenu: [] });
            this.setState({ keyword: value });
            let menu = this.props.defaultKeyword.filter(menu => menu.toLowerCase().indexOf(value.toLowerCase()) > -1);
            this.setState({ keywordMenu: menu });
          }}
          renderMenu={children => <div className='fui-dropdown fui-keyword-menu'>{children}</div>}
          renderItem={(item, isHighlighted) => (
            <div key={`autocompelte-${item}`} className={`fui-item ${isHighlighted ? 'is-selected' : ''}`}>
              <span className='prefix'>
                <i className='icon-search'></i>
              </span>
              <div className='content' dangerouslySetInnerHTML={{ __html: this.highlightKeyword(item) }}></div>
            </div>
          )}
        />
      </div>
    );
  }
}

KeywordInput.propTypes = {
  path: PropTypes.string,
  title: PropTypes.string,
  placeholder: PropTypes.string,
  keyword: PropTypes.string,
  defaultKeyword: PropTypes.arrayOf(PropTypes.string),
};

export default KeywordInput;

```

#### Properties

| 名稱 | 屬性 | 必填 | 選項 | 說明 |
| :--- | :--- | :--- | :--- | :--- |
| path | string |  |  | 轉址位置 |
| title | string |  |  | 標題 |
| placeholder | string |  |  | 預設顯示 |
| keyword | string |  |  | 輸入關鍵字 |
| defaultKeyword | array |  |  | 預設關鍵字陣列 |

### HotWord

**Desktop**
![](../.gitbook/assets/image%20%28123%29.png)
{% endtab %}

**Mobile**
![](../.gitbook/assets/image%20%28218%29.png)

**Usage**
```javascript
import HotWord from '../HotWord';

<HotWord hot={{
  path: '/search',
  hotword: [
    '遠傳物聯網',
    '雲端運算',
    '主機代管',
    '大寬頻企業光纖',
    'Office 365',
    '007 國際電話',
    '世界電話國碼',
    '世界電話國碼',
    '世界電話國碼',
    '世界電話國碼',
  ]
}} />
```

**Source**
```javascript
import React from 'react';
import PropTypes from 'prop-types';
import { Link } from 'react-router-dom';

class HotWord extends React.Component {
  constructor(props) {
    super(props);
    this.keywords = React.createRef();
    this.state = {
      currentTab: 0,
      currentScroll: 0,
      scroller: false,
    };

    this.setScroller = this.setScroller.bind(this);
    this.doScroll = this.doScroll.bind(this);

    window.addEventListener('resize', e => this.setScroller);
  }

  componentDidMount() {
    this.setScroller();
  }

  detectLast() {
    if (!this.keywords.current) return false;
    return (
      this.state.currentScroll + this.keywords.current.clientWidth >= this.keywords.current.children[0].clientWidth
    );
  }

  doScroll = dir => {
    let clientW = this.keywords.current.clientWidth;
    if (window.isIE) {
      if (dir === 'left') this.keywords.current.scrollLeft = this.keywords.current.scrollLeft - clientW;
      if (dir === 'right') this.keywords.current.scrollLeft = this.keywords.current.scrollLeft + clientW;
    } else {
      if (dir === 'left')
        this.keywords.current.scrollTo({
          left: this.keywords.current.scrollLeft - clientW,
          top: 0,
          behavior: 'smooth',
        });
      if (dir === 'right')
        this.keywords.current.scrollTo({
          left: this.keywords.current.scrollLeft + clientW,
          top: 0,
          behavior: 'smooth',
        });
    }
    this.setState({ currentScroll: this.keywords.current.scrollLeft });
  };

  setScroller() {
    let outer = this.keywords.current.clientWidth;
    let inner = this.keywords.current.children[0].clientWidth;

    if (outer < inner) {
      this.setState({ scroller: true });

      this.keywords.current.addEventListener('scroll', e => {
        this.setState({ currentScroll: e.target.scrollLeft });
      });
    }
  }

  render() {
    return (
      <div className='keyword-container'>
        <div
          className={`keyword-list ${this.state.scroller ? 'is-scroller' : ''} ${
            this.state.currentScroll > 0 ? 'is-show-before' : ''
          } ${this.detectLast() ? 'is-hide-after' : ''}`}>
          <div className='label'>熱搜</div>
          <div className='keyword-content' ref={this.keywords}>
            <div className='keyword-group'>
              {this.props.hotword.map((word, i) => (
                <Link
                  role='button'
                  key={`hot-search-${i}`}
                  to={`${this.props.path}?keyword=${word}`}
                  className='fui-button is-label'>
                  {word}
                </Link>
              ))}
            </div>
          </div>
          {this.state.scroller ? (
            <div className='keyword-action'>
              <button
                onClick={e => {
                  this.doScroll('left');
                }}
                className={`fui-button is-text ${this.state.currentScroll === 0 ? 'disabled' : ''}`}>
                <i className='icon-chevron-left'></i>
              </button>
              <button
                onClick={e => {
                  this.doScroll('right');
                }}
                className={`fui-button is-text ${this.detectLast() ? 'disabled' : ''}`}>
                <i className='icon-chevron-right'></i>
              </button>
            </div>
          ) : (
            ''
          )}
        </div>
      </div>
    );
  }
}

HotWord.propTypes = {
  path: PropTypes.string,
  hotword: PropTypes.arrayOf(PropTypes.string),
};

export default HotWord;

```

#### Properties

| 名稱 | 屬性 | 必填 | 選項 | 說明 |
| :--- | :--- | :--- | :--- | :--- |
| path | string |  |  | 搜尋後前往的路徑 |
| hotword | array |  |  | 熱搜列表 |

