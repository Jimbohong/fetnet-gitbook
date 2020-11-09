# RadioCarousel

用於門號選擇，資料格式可帶門號價格

### Usage
```jsx
class Product extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      flow: id: 'new-number',
    slug: '門號',
    title: '選擇門號',
    type: 'radio-carousel',
    name: 'phone_number',
    description: '選一個好門號給你好運氣',
    searchLabel: '主題號碼',
    tags: ['免費門號', ...],
    options: [
        {
          label: '0903-063-482',
          value: '0903-063-482',
          description: '選號費將於第一期帳單進行扣抵',
          price: 1000,
          tag: ['黃金門號'],
        },
        {
          label: '0903-652-344',
          value: '0903-652-344',
          tag: ['黃金門號'],
        },
      ]
    }
  }
  render() {
    return (
      <RadioCarousel
        search={true}
        name={flow.name}
        {...flow}
        disabled={this.state.isBackward}
        value={form[flow.name].value}
        onChange={this.inputChange}
      />
    )
  }
}
```

### Sources
```jsx
import React from 'react';
import { withFormsy } from 'formsy-react';
import Slider from 'react-slick';

import Button from '../Button';
import PropTypes from 'prop-types';

import formatNumber from '../../utils/numberFormatter';

class RadioCarousel extends React.Component {
  constructor(props) {
    super(props);
    this.searchInput = React.createRef();
    this.state = {
      isMobile: window.innerWidth < 1440,
      selected: this.props.value || '',
      keyword: '',
      showTag: false,
      options: this.props.options,
      sliderSetting: {
        dots: false,
        infinite: false,
        arrows: true,
        slidesToShow: 1,
        draggable: true,
        centerPadding: '6px',
        adaptiveHeight: false,
        centerMode: false,
        responsive: [
          {
            breakpoint: 960,
            settings: {
              arrows: true,
              slidesToShow: 1,
            },
          },
        ],
      },
    };
  }

  componentDidMount() {
    if (this.props.disabled) {
      let selectIdx = -1;
      let options = [...this.props.options];

      this.props.options.forEach((opt, i) => {
        if (opt.value === this.state.selected) {
          selectIdx = i;
        }
      });

      let selected = options.splice(selectIdx, 1);
      options = [...selected, ...options];
      this.setState({
        options: options,
      });
    }

    window.addEventListener('resize', (e) => {
      this.setState({
        isMobile: window.innerWidth < 1440,
      });
    });
  }

  componentDidUpdate = (prevProps, prevState) => {
    if (this.props.keyword !== prevProps.keyword) {
      this.setState({
        keyword: this.props.keyword,
      });
    }
    if (this.props.value !== prevProps.value || this.props.value !== prevState.selected) {
      this.setState({
        selected: this.props.value,
      });
    }
  };

  searchNumber = (e) => {
    this.setState({
      keyword: e.currentTarget.value,
    });
  };

  optionFilter = (array) => {
    let key = this.state.keyword;
    if (key === '') return array;

    return array.reduce((accr, val, i, ary) => {
      if (val.label.indexOf(key) > -1 || val.value.indexOf(key) > -1 || val.tag.indexOf(key) > -1) {
        accr.push(val);
      }
      return accr;
    }, []);
  };

  groupNumbers = () => {
    let options = this.optionFilter(this.state.options).reduce((accr, value, index, array) => {
      let i = Math.floor(index / (this.state.isMobile ? 6 : 9));

      if (!accr[i]) accr[i] = [];

      accr[i].push(value);

      return accr;
    }, []);

    return options;
  };

  toggleTag = (toggle) => {
    if (toggle) {
      this.setState({
        showTag: toggle,
      });
    } else {
      setTimeout(() => {
        this.setState({
          showTag: toggle,
        });
      }, 500);
    }
  };

  removeKeyword = (e) => {
    this.searchInput.current.value = '';
    this.setState({
      keyword: '',
    });
  };

  setKeyword = (item) => {
    this.searchInput.current.value = item;
    this.setState({
      keyword: item,
    });
  };

  handleChange = (e, opt) => {
    this.props.setValue(e.currentTarget.value);
    this.props.onChange(this.props.name, e.currentTarget.value, opt);
  };

  render() {
    return (
      <div className='radio-carousel-container'>
        {this.props.search ? (
          <div className='search-input'>
            <div className='input-group'>
              <input
                type='text'
                placeholder='輸入喜愛的關鍵門號號碼'
                disabled={this.props.disabled}
                onFocus={(e) => this.toggleTag(true)}
                onBlur={(e) => this.toggleTag(false)}
                onChange={(e) => this.searchNumber(e)}
                onKeyDown={(e) => this.searchNumber(e)}
                ref={this.searchInput}
              />
              <button
                className={`reset ${this.state.keyword !== '' ? 'd-block' : 'd-none'}`}
                onClick={this.removeKeyword}>
                <i className='icon-close'></i>
              </button>
              <button className='search'>
                <i className='icon-search'></i>
              </button>
            </div>
            <div className={`dropdown-wrapper ${this.state.showTag ? 'd-block' : 'd-none'}`}>
              <p className='mt-0'>{this.props.searchLabel}</p>
              {this.props.tags.map((item, idx) => (
                <Button
                  btnStyle='label'
                  size='small'
                  className={`${this.props.selected && this.props.selected === item ? 'active' : ''} `}
                  key={'menu-item' + idx}
                  onClick={(e) => this.setKeyword(item)}>
                  {item}
                </Button>
              ))}
            </div>
          </div>
        ) : null}
        <Slider {...this.state.sliderSetting} className='fui-radio-carousel'>
          {this.groupNumbers().length ? (
            this.groupNumbers().map((group, i) => (
              <div
                className={`radio-buttons is-number-group ${this.props.disabled ? 'is-disabled' : ''}`}
                key={`number-input-group-${i}`}>
                {group.map((option, j) => (
                  <label className='radio-button' key={`radio-button-${this.props.name}-${option.label}`}>
                    <input
                      type='radio'
                      disabled={this.props.disabled}
                      value={option.value}
                      defaultChecked={option.value === this.state.selected}
                      name={this.props.name}
                      onChange={(e) => this.handleChange(e, option)}
                    />
                    <div className='content'>
                      <div className='phone-number'>
                        {option.label}
                        {option.price ? (
                          <div>
                            <div className='fui-tag'>+${formatNumber(option.price)}</div>
                          </div>
                        ) : null}
                      </div>
                      {option.value === this.state.selected && option.price ? (
                        <div className='text-sm'>費用將於首筆帳單扣抵</div>
                      ) : null}
                    </div>
                    <span className='checkmark'></span>
                  </label>
                ))}
              </div>
            ))
          ) : (
            <div className='no-result'>
              <img src={'/resources/cbu/estore-product/searchnoresult@2x.png'} width='220' alt='搜尋無結果！' />
              <div>搜尋無結果！</div>
            </div>
          )}
        </Slider>
      </div>
    );
  }
}

RadioCarousel.propTypes = {
  name: PropTypes.string,
  keyword: PropTypes.string,
  disabled: PropTypes.bool,
  value: PropTypes.string,
  tags: PropTypes.array,
  search: PropTypes.bool,
  searchLabel: PropTypes.string,
  options: PropTypes.array,
  onChange: PropTypes.func,
};

export default withFormsy(RadioCarousel);
```

### Properties
| 名稱 | 屬性 | 必填 | 選項 | 說明 |
| :--- | :--- | :--- | :--- | :--- |
| name | String | true |  | 欄位名稱 |
| keyword | String |  |  | 搜尋的關鍵字 |
| disabled | Boolean |  |  | disabled 狀態，由結帳流程返回申辦流程時觸發 |
| value | String | true |  | 欄位值 |
| tags | Array | true |  | 關鍵字可搜尋的標籤 |
| search | Boolean | true |  | 是否可搜尋 |
| searchLabel | String | true |  | 搜尋欄位名稱 |
| options | String | true |  | 選項，請參考 Usage |
| onChange | String | true |  | 選擇門號後回傳 |