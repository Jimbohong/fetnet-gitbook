#CheckboxCardCarousel

商品選擇輪播模組，用於申辦流程


![checkbox-card-carousel](/images/checkbox-card-carousel.png)

## Usage 
```jsx
class Product extends React.component {
  constructor(props) {
    super(props);

    this.state = {
      options: [
        {
          type: 'product',
          image: '/resources/cbu/estore-product/estore-product-thumb-12@2x.jpg',
          value: 'VANTEC Qi 紅色無線快速充電盤',
          meta: 'VANTEC Qi',
          name: 'VANTEC Qi 紅色無線快速充電盤',
          originPrice: 2400,
          productPrice: 0,
          modal: {
            title: 'VANTEC Qi 紅色無線快速充電盤',
            content: [
              '商品規格',
              [
                '3.5mm 超薄無線充電盤',
                'WPC國際 Qi 認證/ NCC 國家認證',
                '支持Qi 無線充電IOS & Android 手機',
                'FOD自動智慧識別金屬異物',
                '輸出 (Output)：5W/ 7.5W / 9W',
              ],
            ],
          },
        },
        ...
      ],
      form: {
        gift: { value: '', required: true },
      },
    }
  }

  render () {
    return (
      <CheckboxCardCarousel
        name={flow.name}
        minLength={0}
        maxLength={2}
        name='gift'
        options={this.state.options}
        value={form.addon.value}
        onChange={this.inputChange}
        modalOpen={this.openModal}
      />
    )
  }
}
```

## Sources
```jsx
import React from 'react';
import Slider from 'react-slick';
import PropTypes from 'prop-types';
import { withFormsy } from 'formsy-react';
import NumberSelector from './NumberSelector';
import formatNumber from '../../utils/numberFormatter';

class CheckboxCardCarousel extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      currentTag: '全部',
      selected: this.props.value ? this.props.value.split(/,/g) : [],
      defaultChecked: this.props.value ? this.props.value.split(/,/g) : [],
      options: [],
      sliderSetting: {
        dots: false,
        infinite: false,
        arrows: true,
        draggable: true,
        adaptiveHeight: true,
        slidesToShow: 4,
        centerMode: false,
        responsive: [
          {
            breakpoint: 1024,
            settings: {
              arrows: true,
              slidesToShow: 3,
              variableWidth: true,
            },
          },
          {
            breakpoint: 768,
            settings: {
              arrows: true,
              slidesToShow: 2,
              variableWidth: true,
            },
          },
          {
            breakpoint: 600,
            settings: {
              arrows: true,
              slidesToShow: 1,
              variableWidth: true,
            },
          },
        ],
      },
    };
  }

  componentDidMount() {
    if (this.props.default) {
      this.props.setValue(this.props.value);
    }

    if (this.props.withNumberController) {
      this.setState({
        options: this.props.options.reduce((accr, val, idx, arr) => {
          accr.push({
            ...val,
            number: 1,
          });
          return accr;
        }, []),
      });
    } else {
      this.setState({
        options: this.props.options,
      });
    }
  }

  componentDidUpdate(prevProps) {
    if (prevProps.value !== this.props.value) {
      this.setState({
        defaultChecked: this.props.value ? this.props.value.split(/,/g) : [],
        selected: this.props.value ? this.props.value.split(/,/g) : [],
      });
    }
  }

  numberChange = (i, v) => {
    let opts = Object.assign(this.state.options);
    opts[i].number = v;
    this.setState({
      options: opts,
    });

    let selected = Object.assign(this.state.selected);
    let objects = opts.reduce((accr, obj, index, array) => {
      if (selected.indexOf(obj.value) > -1) accr.push(obj);

      return accr;
    }, []);

    this.props.onChange(this.props.name, selected.toString(), objects);
  };

  handleChange = (event) => {
    let selected = Object.assign(this.state.selected);
    let val = event.currentTarget.value;
    if (event.currentTarget.checked) {
      selected.push(val);
    } else {
      selected.splice(selected.indexOf(val), 1);
    }
    // debugger
    this.setState({ selected: selected });
    this.props.setValue(selected.length ? selected.toString() : null);

    let objects = this.state.options.reduce((accr, obj, index, array) => {
      if (selected.indexOf(obj.value) > -1) accr.push(obj);

      return accr;
    }, []);

    this.props.onChange(this.props.name, selected.toString(), objects);
  };

  filterOption = (option) => {
    if (this.state.currentTag === '全部') return this.state.options;
    else {
      return this.state.options.filter(
        (opt) =>
          (opt.name && opt.name.toLowerCase().indexOf(this.state.currentTag.toLowerCase()) > -1) ||
          (opt.title && opt.title.toLowerCase().indexOf(this.state.currentTag.toLowerCase()) > -1) ||
          (opt.meta && opt.meta.toLowerCase().indexOf(this.state.currentTag.toLowerCase()) > -1)
      );
    }
  };

  getDefaultCheck = (option) => {
    let isCheck = this.state.defaultChecked.indexOf(option.value) > -1;
    // console.log(this.state.defaultChecked, option, isCheck);
    return isCheck;
  };

  render() {
    const { minLength, maxLength } = this.props;
    const { selected } = this.state;

    return (
      <div className='fui-checkbox-cards-container'>
        {this.props.tag ? (
          <div className='tag-filter'>
            <div className='content'>
              <div className='tag-list'>
                <label
                  className={`fui-button is-label ${this.state.currentTag === '全部' ? 'is-active' : ''}`}
                  role='button'
                  onClick={(e) =>
                    this.setState({
                      currentTag: '全部',
                    })
                  }>
                  <span className='text'>全部({this.state.options.length})</span>
                </label>
                {this.props.tag.map((tag, i) => (
                  <label
                    className={`fui-button is-label ${this.state.currentTag === tag ? 'is-active' : ''}`}
                    role='button'
                    key={`menu-tag-${i}`}
                    onClick={(e) =>
                      this.setState({
                        currentTag: tag,
                      })
                    }>
                    <span className='text'>{tag}</span>
                  </label>
                ))}
              </div>
            </div>
          </div>
        ) : null}

        {this.filterOption().length ? (
          <Slider {...this.state.sliderSetting} className='fui-checkbox-cards-slider'>
            {this.filterOption().map((opt, i) => (
              <div
                className={`fui-card is-${opt.type === 'product' ? 'product' : 'plan'} is-checkbox ${
                  this.getDefaultCheck(opt) ? 'is-checked' : ''
                }`}
                key={`checkcard-${opt.value}`}>
                <label className='fui-card-action'>
                  <input
                    type='checkbox'
                    name={this.props.name}
                    value={opt.value}
                    disabled={
                      minLength &&
                      minLength !== -1 &&
                      selected.length > 0 &&
                      selected.length >= minLength &&
                      !this.getDefaultCheck(opt)
                    }
                    onChange={(e) => this.handleChange(e)}
                    defaultChecked={this.getDefaultCheck(opt)}
                  />
                  {opt.type === 'product' ? (
                    <div className='fui-card-image'>
                      <img
                        src={opt.image || process.env.PUBLIC_URL + '/resources/common/images/no-image@2x.png'}
                        alt={opt.title || opt.name}
                      />
                    </div>
                  ) : null}
                  {opt.type === 'friday' ? (
                    <div className='fui-card-plan is-friday'>
                      <div>
                        <h1>${formatNumber(opt.productPrice)}</h1>
                        <h4>{opt.meta}</h4>
                      </div>
                    </div>
                  ) : null}
                  {opt.type === 'music' ? (
                    <div className='fui-card-plan is-music'>
                      <div>
                        <h1>${formatNumber(opt.productPrice)}</h1>
                        <h4>{opt.meta}</h4>
                      </div>
                    </div>
                  ) : null}

                  <div className='fui-card-caption'>
                    {opt.type === 'product' ? <div className='fui-card-meta'>{opt.meta}</div> : null}
                    <div className='fui-card-title'>{opt.name || opt.title}</div>
                    <div className='fui-card-price'>
                      <div>
                        <span className='label'>{opt.type === 'product' ? '商品' : '加購'}價</span>
                        {opt.originPrice ? <del>${formatNumber(opt.originPrice)}</del> : null}
                        <b className={`price ${opt.originPrice ? 'is-text-error' : ''}`}>
                          ${formatNumber(opt.productPrice)}
                          {opt.day || opt.unit ? '/' : ''}
                          {opt.day || ''}
                          {opt.unit || ''}
                        </b>
                      </div>
                    </div>
                  </div>
                </label>
                {opt.modal ? (
                  <div className='fui-card-extra'>
                    <div role='button' className='is-text-underline' onClick={(e) => this.props.modalOpen(opt.modal)}>
                      規格說明
                    </div>
                  </div>
                ) : null}
                {this.props.withNumberController ? (
                  <NumberSelector
                    name={this.props.name + '-number'}
                    min={1}
                    value={opt.number}
                    showNote={false}
                    onChange={(n, v) => this.numberChange(i, v)}
                  />
                ) : null}
              </div>
            ))}
          </Slider>
        ) : (
          <div className='no-result'>
            <img src={'/resources/cbu/estore-product/searchnoresult@2x.png'} width='220' alt='搜尋無結果！' />
            <div>搜尋無結果！</div>
          </div>
        )}
      </div>
    );
  }
}

CheckboxCardCarousel.propTypes = {
  minLength: PropTypes.number,
  maxLength: PropTypes.number,
  name: PropTypes.string,
  options: PropTypes.array,
  value: PropTypes.string,
  onChange: PropTypes.func,
  modalOpen: PropTypes.func,
  withNumberController: PropTypes.bool,
};

export default withFormsy(CheckboxCardCarousel);

```


### Properties

| 名稱 | 屬性 | 必填 | 選項 | 說明 |
| :--- | :--- | :--- | :--- | :--- |
| name | String | true |  | 輸入框名稱 |
| value | String | true |  | 回傳選擇的值，如：“1,2,3” |
| minLength | Number |  |  | 最少選取數量 |
| maxLength | Number |  |  | 最大選取數量 |
| options | Array |  |  | 請參閱 Usage |
| withNumberController | Boolean |  |  | 牌卡是否能選擇數量 |
| onChange | Function |  |  | 選取或數量更新異動回傳 |
| modalOpen | String |  |  | 傳入跳窗內容與屬性 |

